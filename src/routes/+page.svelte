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
    <div style="width: 100%; height: 100%; background-color: lightgrey;">
        <h1>Demo Content</h1>
        <p>Draw bounding boxes over this content.</p>
    </div>
</BoundingBox>


<pre>
    {JSON.stringify(boxes, null, 2)}
    </pre>