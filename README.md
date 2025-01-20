# Svelte Bounding Box

This Svelte component allows you to draw, delete, and resize bounding boxes over any child component.

## Installation

1. Clone the repository:
    ```sh
    git clone https://github.com/your-username/svelte-bounding-box.git
    ```

2. Navigate to the project directory:
    ```sh
    cd svelte-bounding-box
    ```

3. Install dependencies:
    ```sh
    npm install
    ```

## Usage

1. Import the `BoundingBox` component and the interfaces in your Svelte file:
    ```svelte
    <script>
        import BoundingBox from "$lib/BoundingBox.svelte";
        import type { Coordinates, Dimensions, Box } from "$routes/+page.svelte";
    </script>
    ```

2. Wrap the content you want to draw bounding boxes over with the `BoundingBox` component:
    ```svelte
    <BoundingBox>
        <div style="width: 100%; height: 100%; background-color: lightgrey;">
            <h1>Demo Content</h1>
            <p>Draw bounding boxes over this content.</p>
        </div>
    </BoundingBox>
    ```

## Features

- **Draw**: Click and drag to draw a bounding box.
- **Delete**: Double-click on a bounding box to delete it.
- **Resize**: Click and drag the edges of a bounding box to resize it.

## How to Draw Boxes

1. Click and hold the mouse button on the area where you want to start drawing the box.
2. Drag the mouse to the desired size of the box.
3. Release the mouse button to finish drawing the box.

## Example

Here is a complete example:

```svelte
<script>
    import BoundingBox from "$lib/BoundingBox.svelte";
    import type { Coordinates, Dimensions, Box } from "$routes/+page.svelte";
</script>

<BoundingBox>
    <div style="width: 100%; height: 100%; background-color: lightgrey;">
        <h1>Demo Content</h1>
        <p>Draw bounding boxes over this content.</p>
    </div>
</BoundingBox>
```

## License

This project is licensed under the MIT License.
