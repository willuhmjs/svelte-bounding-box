<script lang="ts">
    import { onMount, onDestroy } from 'svelte';

    type Coordinates = { x1: number; y1: number; x2: number; y2: number };

	let { children, coordinatesBoxes = $bindable([]), outerColor = $bindable('rgb(255, 62, 0)'), innerColor = $bindable('rgba(255, 62, 0, 0.2)')} = $props();

    let drawing = $state(false);
    let currentCoordinates: Coordinates | null = $state(null);
    let startX: number;
    let startY: number;
    let container
    const MIN_SIZE = 5;

    function isInsideBox(box: Coordinates, x: number, y: number): boolean {
        return x >= box.x1 && x <= box.x2 && y >= box.y1 && y <= box.y2;
    }

    function startDrawing(event) {
        event.preventDefault();
        const { clientX, clientY } = event;
        const rect = container.getBoundingClientRect();
        startX = clientX - rect.left;
        startY = clientY - rect.top;

        for (let i = 0; i < coordinatesBoxes.length; i++) {
            if (isInsideBox(coordinatesBoxes[i], startX, startY)) {
                coordinatesBoxes = coordinatesBoxes.filter((_, index) => index !== i);
                return;
            }
        }

        drawing = true;
        currentCoordinates = { x1: startX, y1: startY, x2: startX, y2: startY };
        coordinatesBoxes = [...coordinatesBoxes, currentCoordinates];
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

        if (currentCoordinates) {
            currentCoordinates.x1 = left;
            currentCoordinates.y1 = top;
            currentCoordinates.x2 = right;
            currentCoordinates.y2 = bottom;
        }

        coordinatesBoxes = [...coordinatesBoxes];
    }

    function stopDrawing() {
        if (currentCoordinates && (Math.abs(currentCoordinates.x2 - currentCoordinates.x1) < MIN_SIZE || Math.abs(currentCoordinates.y2 - currentCoordinates.y1) < MIN_SIZE)) {
            coordinatesBoxes = coordinatesBoxes.slice(0, -1);
        }
        drawing = false;
        currentCoordinates = null;
    }

    onMount(() => {
        if (typeof window !== 'undefined') {
            window.addEventListener('mousemove', draw);
            window.addEventListener('mouseup', stopDrawing);
        }
    });

    onDestroy(() => {
        if (typeof window !== 'undefined') {
            window.removeEventListener('mousemove', draw);
            window.removeEventListener('mouseup', stopDrawing);
        }
    });
</script>

<style>
    .bounding-box {
        position: absolute;
        border: 2px solid var(--outer-color);
        background-color: var(--inner-color);
    }
</style>

<div bind:this={container} onmousedown={startDrawing} style="position: relative; width: 100%; height: 100%;">
    {@render children?.()}
    {#each coordinatesBoxes as box, index}
        <div
            class="bounding-box"
            style="left: {box.x1}px; top: {box.y1}px; width: {box.x2 - box.x1}px; height: {box.y2 - box.y1}px; --outer-color: {outerColor}; --inner-color: {innerColor};"
        ></div>
    {/each}
</div>
