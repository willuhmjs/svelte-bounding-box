# Svelte Bounding Box

This Svelte component allows you to draw, delete, and resize bounding boxes over any child component.

## Installation

1. Clone the repository:
    ```
    npm i svelte-bounding-box
    ```

## Usage

1. Import the `BoundingBox` component and the interfaces in your Svelte file:
    ```svelte
    <script>
        import BoundingBox from "BoundingBox";
    </script>
    ```

2. Wrap the content you want to draw bounding boxes over with the `BoundingBox` component:
    ```svelte
    <BoundingBox bind:boxes>
        <div style="width: 100%; height: 100%; background-color: lightgrey;">
            <h1>Demo Content</h1>
            <p>Draw bounding boxes over this content.</p>
        </div>
    </BoundingBox>
    ```

## Features

- **Draw**: Click and drag to draw a bounding box.
- **Delete**: Click on a bounding box to delete it.

## Example

Here is a complete example:

```svelte
<script>
    import BoundingBox from "$lib/BoundingBox.svelte";
</script>

<BoundingBox bind:boxes>
    <div style="width: 100%; height: 100%; background-color: lightgrey;">
        <h1>Demo Content</h1>
        <p>Draw bounding boxes over this content.</p>
    </div>
</BoundingBox>
```