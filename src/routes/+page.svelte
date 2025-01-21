<script lang="ts">
    import BoundingBox from "$lib/BoundingBox.svelte";

    interface Coordinates {
        x1: number;
        y1: number;
        x2: number;
        y2: number;
    }

    interface Dimensions {
        x: number;
        y: number;
        width: number;
        height: number;
    }

    let dimensionsBoxes: Dimensions[] = $state([]);
    let coordinatesBoxes: Coordinates[] = $state([]);
</script>

<BoundingBox bind:dimensionsBoxes={dimensionsBoxes} bind:coordinatesBoxes={coordinatesBoxes}>
    <div class="content">
        <h1>Demo Content</h1>
        <p>Draw bounding boxes over this content.</p>
    </div>
</BoundingBox>

<div class="pre-container">
    <pre>{JSON.stringify(dimensionsBoxes, null, 2)}</pre>
    <pre>{JSON.stringify(coordinatesBoxes, null, 2)}</pre>
</div>

<style>
    .content {
        padding: 20px;
        border: 1px solid #ccc;
        background-color: #f9f9f9;
    }
    
    :global(body) {
        font-family: sans-serif;
    }

    .pre-container {
        display: flex;
        gap: 10px;
    }

    pre {
        background-color: #f0f0f0;
        padding: 10px;
        border: 1px solid #ccc;
        flex: 1;
    }
</style>