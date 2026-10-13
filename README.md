# ESPHome Addressable LED YAML Generator

Create static effects for ESPHome addressable LED strips, matrices and segmented displays.

The generator runs entirely in your browser and produces ready-to-paste `addressable_lambda` YAML for ESPHome.

GitHub page: [https://micolios.github.io/esphome-led-yaml/](https://micolios.github.io/esphome-led-yaml/)

## Features

- Single-strip, segmented-strip, and square/rectangle layouts
- Per-LED colour selection with click and drag selection
- Left-to-right or right-to-left strip direction
- Selectable starting corner for the grid layout
- Separate colours and selections for each layout mode
- Rainbow, gradient, and custom segment colours
- Image-to-grid conversion with crop, fit, and stretch options
- Scrollable, wrapping, or fit-to-window LED preview
- Adjustable LED preview size, with an optional fixed size
- YAML import/export using embedded generator metadata
- One-click copy to clipboard

## Modes

### Single strip

Use for a normal, non-segmented addressable LED strip.

1. Select **Single strip**.
2. Enter the total number of LEDs.
3. Select LEDs, choose a colour, then select **Apply to selection** or **Apply to all**.
4. Copy the generated YAML into ESPHome.

<details>
<summary><strong>Features and options</strong></summary>

- Enter any strip length
- Choose left-to-right or right-to-left strip direction
- Click or click-and-drag to select LEDs
- Apply a colour to the selection or to every LED
- Enter colours with the picker or a hex value such as `#91cefa`
- Clear the selection or reset all LEDs to black
- Preview shows each LED’s physical strip index

</details>

### Segmented strip

Use for repeating colour bands, flags, indicators, or simple patterns.

1. Select **Segmented strip**.
2. Enter the physical strip length and number of divisions.
3. Select **Divide evenly**.
4. Adjust each segment’s colour or LED count if required.

Alternatively, add or remove segments and set their lengths individually.

The generator creates one compact `it.range()` instruction per segment:

```yaml
it.range(0, 3) = Color(255, 0, 0);
it.range(3, 6) = Color(255, 127, 0);
it.range(6, 9) = Color(255, 255, 0);
```

<details>
<summary><strong>Features and options</strong></summary>

- Add, delete, and individually edit segments
- Set each segment’s LED count and colour
- **Apply strip length** - redistribute existing segments across a new strip length
- **Apply rainbow** - apply the default rainbow palette
- **Apply gradient** - blend between two chosen colours
- **Apply rainbow gradient** - create a smooth red-to-purple rainbow
- **Clear all segments** - remove every segment
- **Reset to default** - restore the seven colour default layout
- Choose left-to-right or right-to-left strip direction

</details>

<details>
<summary><strong>Uneven division</strong></summary>

If the strip length does not divide exactly, remaining LEDs are distributed evenly across the strip by default.

For example, 23 LEDs divided into seven segments produces:

```text
3, 4, 3, 3, 3, 4, 3
```

</details>

### Square / rectangle

Use for LED matrices, panels, and grids.

1. Select **Square / rectangle**.
2. Enter the grid width and height.
3. Configure the wiring direction, serpentine setting, and starting corner.
4. Select LEDs and apply colours, or upload an image.
5. Copy the generated YAML into ESPHome.

<details>
<summary><strong>Features and options</strong></summary>

- Configure grid width and height
- Choose row-by-row or column-by-column data direction
- Enable serpentine / zig-zag wiring
- Choose the starting corner
- Allow larger grids, up to 200 × 200
- Click or click and drag to select LEDs
- Apply colours to the selection or to every LED
- Upload an image and apply its colours to the grid
- Choose fit to window or scrollable preview

</details>

<details>
<summary><strong>Image to grid</strong></summary>

Upload an image and apply its colours to the current grid.

- **Fit with black bars** - preserves aspect ratio and fills unused LEDs with black
- **Stretch to grid** - fills every LED, ignoring the original aspect ratio
- **Crop image** - drag the crop edges to select part of the image
- **Apply image to existing grid** - samples the image at the current grid resolution

The image is sampled once per LED, so a 16 × 16 grid produces 256 colours.

Crop notes:

- Crop positions are stored as percentages of the image
- Unticking **Crop image** ignores the previous crop and uses the full image
- The crop selection resets when cropping is disabled
- Crop handles can be dragged from the left, right, top, and bottom edges

</details>

<details>
<summary><strong>Wiring examples</strong></summary>

A 5 × 2 matrix using serpentine rows:

```text
0  1  2  3  4
9  8  7  6  5
```

The same matrix using serpentine columns:

```text
0  3  4  7  8
1  2  5  6  9
```

</details>

## LED preview

The preview shows each LED’s display position and physical strip index.

- **Wrap LEDs / Scroll horizontally** - choose whether long strips wrap onto new rows or remain in one scrollable row
- **Keep LED size** - keep LEDs at 64 px instead of shrinking them
- **Fit preview to window** - grid mode only; scales the entire grid to fit the available width

By default, preview LEDs range from 32 px to 64 px. In grid fit mode, they can shrink further so the full matrix remains visible; LED numbers are hidden automatically once a box becomes too small.

## Generated YAML

The tool creates an ESPHome `addressable_lambda` effect.

```yaml
effects:
  - addressable_lambda:
      name: "Custom Static"
      update_interval: 1s
      lambda: |-
        it[0] = Color(255, 0, 0);
```

Paste this under the `effects:` section of an ESPHome addressable light.

<details>
<summary><strong>Import and export metadata</strong></summary>

Each generated YAML begins with `# generator-*` comments. These preserve:

- Layout mode and dimensions
- Strip direction and grid wiring options
- Segment definitions
- LED colours

Use **Import YAML** to paste a previously generated file back into the tool. Files without generator metadata cannot be imported.

</details>

<details>
<summary><strong>Grid size and performance</strong></summary>

- Grids are limited to 64 × 64 by default
- Enable **Allow larger grids** to permit up to 200 × 200
- Large grids can slow down preview rendering and YAML generation
- 64 × 64 is a practical upper limit for most devices

</details>

## Usage

Visit [https://micolios.github.io/esphome-led-yaml/](https://micolios.github.io/esphome-led-yaml/)

Or:

1. Open `index.html` in a browser.
2. Choose a layout mode.
3. Set the LED count or grid dimensions.
4. Select LEDs and apply colours, or import an image.
5. Copy the generated YAML into ESPHome.
