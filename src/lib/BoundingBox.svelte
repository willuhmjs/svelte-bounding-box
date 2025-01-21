<script lang="ts">
    import { onMount, onDestroy } from 'svelte';

    type Dimensions = { x: number; y: number; width: number; height: number };
    type Coordinates = { x1: number; y1: number; x2: number; y2: number };

	let { children, dimensionsBoxes = [], coordinatesBoxes = [], outerColor = 'rgb(255, 62, 0)', innerColor = 'rgba(255, 62, 0, 0.2)'} = $props();



    let drawing = $state(false);
    let currentDimensions: Dimensions | null = $state(null);
    let currentCoordinates: Coordinates | null = $state(null);
    let startX: number;
    let startY: number;
    let container
    const MIN_SIZE = 5;

    function isInsideBox(box: Dimensions, x: number, y: number): boolean {
        return x >= box.x && x <= box.x + box.width && y >= box.y && y <= box.y + box.height;
    }

    function startDrawing(event) {
        event.preventDefault();
        const { clientX, clientY } = event;
        const rect = container.getBoundingClientRect();
        startX = clientX - rect.left;
        startY = clientY - rect.top;

        for (let i = 0; i < dimensionsBoxes.length; i++) {
            if (isInsideBox(dimensionsBoxes[i], startX, startY)) {
                dimensionsBoxes = dimensionsBoxes.filter((_, index) => index !== i);
                coordinatesBoxes = coordinatesBoxes.filter((_, index) => index !== i);
                return;
            }
        }

        drawing = true;
        currentDimensions = { x: startX, y: startY, width: 0, height: 0 };
        currentCoordinates = { x1: startX, y1: startY, x2: startX, y2: startY };
        dimensionsBoxes = [...dimensionsBoxes, currentDimensions];
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

        if (currentDimensions && currentCoordinates) {
            currentDimensions.x = left;
            currentDimensions.y = top;
            currentDimensions.width = right - left;
            currentDimensions.height = bottom - top;

            currentCoordinates.x1 = left;
            currentCoordinates.y1 = top;
            currentCoordinates.x2 = right;
            currentCoordinates.y2 = bottom;
        }

        dimensionsBoxes = [...dimensionsBoxes];
        coordinatesBoxes = [...coordinatesBoxes];
    }

    function stopDrawing() {
        if (currentDimensions && (Math.abs(currentDimensions.width) < MIN_SIZE || Math.abs(currentDimensions.height) < MIN_SIZE)) {
            dimensionsBoxes = dimensionsBoxes.slice(0, -1);
            coordinatesBoxes = coordinatesBoxes.slice(0, -1);
        }
        drawing = false;
        currentDimensions = null;
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
    {#each dimensionsBoxes as box, index}
        <div
            class="bounding-box"
            style="left: {box.x}px; top: {box.y}px; width: {box.width}px; height: {box.height}px; --outer-color: {outerColor}; --inner-color: {innerColor};"
        ></div>
    {/each}
</div>
