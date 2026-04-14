# Design Envelopes

A browser-based tool for computing dead load and pattern-loaded live load design envelopes on simply-supported beams with optional cantilevers on both ends. Envelopes are generated at 2 ft spacing along the bridge using influence line integration.

## Author

Developed by Christopher Higgins, PhD, PE  
School of Civil and Construction Engineering  
Oregon State University, 2025

Based on original Fortran analysis program by C. Higgins (2004)

## What It Does

For each point at 2 ft spacing along the bridge, this program computes the full unit-load influence line and integrates it to determine design envelope values:

- **Dead load moment and shear** — the dead load covers the entire bridge, so the full influence line is integrated with the dead load intensity to produce a single M and V value at each point
- **Live load moment and shear envelopes** — the live load is pattern-loaded using the influence line: positive IL areas are loaded independently from negative IL areas to produce the maximum positive and maximum negative effects at each point. This gives four envelope values: +M, −M, +V, −V

The output is four envelope plots (dead load moment, dead load shear, live load moment envelope, live load shear envelope) and a data table with all values at 2 ft spacing.

## How to Use It

1. **Open `design-envelopes.html` in any modern web browser** (Chrome, Edge, Firefox, Safari). No installation required.
2. **Define the beam geometry**: enter the main span length and optional left/right cantilever lengths in feet.
3. **Set load intensities**: live load and dead load in kips per foot (both default to 1.0 k/ft).
4. **Click Run Analysis**. The computation may take a few seconds as it runs a separate influence line analysis at each 2 ft point.

## Results Tabs

- **Envelope Plots** — Four full-width plots stacked vertically: Dead Load Moment, Dead Load Shear, Live Load Moment Envelope (max positive in green, max negative in red, with shaded fill between), and Live Load Shear Envelope. Below the plots, summary cards show the global extreme values and their locations along the bridge.
- **Data Table** — Scrollable table at 2 ft spacing with seven columns: position, dead M, dead V, live +M, live −M, live +V, live −V. At supports (where cantilevers meet the main span), the table includes rows at ±0.0001 ft from the support to capture the shear discontinuity, highlighted in orange with bold shear values.

## Pattern Loading Concept

The live load envelope is computed by selectively loading only the portions of the bridge that produce the worst-case effect at each point:

- **Maximum positive moment at a point**: live load is placed only where the moment influence line is positive
- **Maximum negative moment at a point**: live load is placed only where the moment influence line is negative
- **Maximum positive/negative shear**: same concept applied to the shear influence line

This is done automatically by integrating the positive and negative areas of the influence line separately.

## Support Discontinuities

When cantilevers are present, shear has a discontinuity at each support (where the cantilever meets the main span). The program handles this by:

- Skipping the exact support location in the 2 ft grid
- Computing values at ±0.0001 ft from the support to capture the shear on both sides
- Highlighting these rows in the data table

When there are no cantilevers, the supports are at the bridge ends and are handled by a small offset (0.0001 ft) from each end.

## Sign Convention

- **Positive moment**: deck in compression (sagging)
- **Positive shear**: standard beam convention

## Technical Details

- At each 2 ft point, a complete unit-load influence line is computed using 1000 increments across the bridge
- The influence line is integrated using the trapezoidal rule with zero-crossing detection to separate positive and negative areas
- For a typical 80 ft bridge, this involves approximately 40 separate influence line analyses
- Analysis points near span boundaries use a small offset (0.0001 ft) to avoid numerical singularities

## Requirements

- A modern web browser (Chrome, Edge, Firefox, or Safari)
- An internet connection is needed the first time you open the file to load fonts and the React library from CDN. After that, your browser cache handles it.

## License

MIT License — see [LICENSE](LICENSE) for details.

## Disclaimer

This software is provided as-is for educational and reference purposes. Engineers should independently verify all results before using them for design. The author assumes no liability for the use of this software in engineering practice.
