<script lang="ts">
    import BoundingBox from "$lib/BoundingBox.svelte";

    interface Box {
        x: number;
        y: number;
        width: number;
        height: number;
    }

    let boxes: Box[] = [];
</script>

<BoundingBox bind:boxes={boxes}>
    <div class="content">
        <h1>Demo Content</h1>
        <p>Draw bounding boxes over this content.</p>
    </div>
</BoundingBox>

<pre style="background-color: #f0f0f0; padding: 10px; border: 1px solid #ccc;">
    {JSON.stringify(boxes, null, 2)}
</pre>
<style>
    .content {
        padding: 20px;
        border: 1px solid #ccc;
        background-color: #f9f9f9;
    }
    
    :global(body) {
        font-family: sans-serif;
    }
</style>