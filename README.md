# svelte-bounding-box

### Drawing Bounding Boxes

1. Click and drag on the content area to draw a bounding box.
2. Click inside an existing bounding box to remove it.

### Customization

You can customize the appearance of the bounding boxes by modifying the `outerColor` and `innerColor` props of the `BoundingBox` component.

Example:
```svelte
<BoundingBox
    bind:coordinatesBoxes={coordinatesBoxes}
    outerColor="rgb(0, 123, 255)"
    innerColor="rgba(0, 123, 255, 0.2)"
>
    <div class="content">
        <h1>Demo Content</h1>
        <p>Draw bounding boxes over this content.</p>
    </div>
</BoundingBox>
```

## License

This project is licensed under the MIT License.
