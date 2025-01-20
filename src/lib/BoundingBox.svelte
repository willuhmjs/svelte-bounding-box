<script lang="ts">
    import { onMount } from 'svelte';
    import type { Box } from '../routes/+page.svelte';

    export let boxes: Box[] = [];
    export let outerColor: string = 'rgb(255,62,0)';
    export let innerColor: string = 'rgba(255,62,0,0.2)';
    let drawing = false;
    let currentBox: Box | null = null;
    let startX: number;
    let startY: number;
    let container;
    const MIN_SIZE = 5;

    function isInsideBox(box: Box, x: number, y: number): boolean {
        return x >= box.dimensions.x && x <= box.dimensions.x + box.dimensions.width && y >= box.dimensions.y && y <= box.dimensions.y + box.dimensions.height;
    }

    function startDrawing(event) {
        event.preventDefault();
        const { clientX, clientY } = event;
        const rect = container.getBoundingClientRect();
        startX = clientX - rect.left;
        startY = clientY - rect.top;

        for (let i = 0; i < boxes.length; i++) {
            if (isInsideBox(boxes[i], startX, startY)) {
                boxes = boxes.filter((_, index) => index !== i);
                return;
            }
        }

        drawing = true;
        currentBox = { dimensions: { x: startX, y: startY, width: 0, height: 0 }, coordinates: { x1: startX, y1: startY, x2: startX, y2: startY } };
        boxes = [...boxes, currentBox];
    }

    function draw(event) {
        event.preventDefault();
        if (!drawing) return;
        const { clientX, clientY } = event;
        const rect = container.getBoundingClientRect();
        const currentX = Math.max(0, Math.min(clientX - rect.left, rect.width));
        const currentY = Math.max(0, Math.min(clientY - rect.top, rect.height));

        const left = Math.min(startX, currentX);
        const top = Math.min(startY, currentY);
        const right = Math.max(startX, currentX);
        const bottom = Math.max(startY, currentY);

        currentBox.dimensions.x = left;
        currentBox.dimensions.y = top;
        currentBox.dimensions.width = right - left;
        currentBox.dimensions.height = bottom - top;

        currentBox.coordinates.x1 = left;
        currentBox.coordinates.y1 = top;
        currentBox.coordinates.x2 = right;
        currentBox.coordinates.y2 = bottom;

        boxes = [...boxes];
    }

    function stopDrawing() {
        if (currentBox && (Math.abs(currentBox.dimensions.width) < MIN_SIZE || Math.abs(currentBox.dimensions.height) < MIN_SIZE)) {
            boxes = boxes.slice(0, -1);
        }
        drawing = false;
        currentBox = null;
    }

    onMount(() => {
        window.addEventListener('mousemove', draw);
        window.addEventListener('mouseup', stopDrawing);
    });
</script>

<style>
    .bounding-box {
        position: absolute;
        border: 2px solid var(--outer-color);
        background-color: var(--inner-color);
    }
</style>

<div bind:this={container} on:mousedown={startDrawing} style="position: relative; width: 100%; height: 100%;">
    <slot></slot>
    {#each boxes as box, index}
        <div
            class="bounding-box"
            style="left: {box.dimensions.x}px; top: {box.dimensions.y}px; width: {box.dimensions.width}px; height: {box.dimensions.height}px; --outer-color: {outerColor}; --inner-color: {innerColor};"
        ></div>
    {/each}
</div>
