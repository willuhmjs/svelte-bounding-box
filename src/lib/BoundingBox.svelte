<script lang="ts">
    import { onMount } from 'svelte';

    interface Box {
        x: number;
        y: number;
        width: number;
        height: number;
    }

    export let boxes: Box[] = [];
    let drawing = false;
    let currentBox: Box | null = null;
    let startX: number;
    let startY: number;
    let container;
    const MIN_SIZE = 5;

    function isInsideBox(box: Box, x: number, y: number): boolean {
        return x >= box.x && x <= box.x + box.width && y >= box.y && y <= box.y + box.height;
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
        currentBox = { x: startX, y: startY, width: 0, height: 0 };
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

        currentBox.x = left;
        currentBox.y = top;
        currentBox.width = right - left;
        currentBox.height = bottom - top;

        boxes = [...boxes];
    }

    function stopDrawing() {
        if (currentBox && (Math.abs(currentBox.width) < MIN_SIZE || Math.abs(currentBox.height) < MIN_SIZE)) {
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
        border: 2px solid red;
        background-color: rgba(255, 0, 0, 0.2);
    }
</style>

<div bind:this={container} on:mousedown={startDrawing} style="position: relative; width: 100%; height: 100%;">
    <slot></slot>
    {#each boxes as box, index}
        <div
            class="bounding-box"
            style="left: {box.x}px; top: {box.y}px; width: {box.width}px; height: {box.height}px;"
        ></div>
    {/each}
</div>
