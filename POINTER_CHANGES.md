# Trackball input processors

The existing 43-key physical layout, bindings, and layer order are unchanged.
Layer 4 remains MOUSE and layer 5 remains SCROLL.

On the right half, the PMW3610 now always reports X/Y motion. The ZMK input
listener activates MOUSE for 700 ms after movement, except for 200 ms after
typing. Pressing a regular key deactivates MOUSE. The existing J/K/L mouse
button positions (18/19/20) are excluded from that deactivation, and a mouse
button press refreshes the 700 ms timer.

While SCROLL is active, its listener override maps Y motion to vertical scroll,
adds inertia, and scales the result. The override replaces the pointer chain
for that layer, so movement in SCROLL does not activate MOUSE. The old PMW3610
scroll mode and automouse mode have been removed from the keymap; both would
otherwise bypass or conflict with the input processors.

## Adjustments

- Layer numbers: `ROBA_MOUSE_LAYER` and `ROBA_SCROLL_LAYER` in
  `boards/shields/roBa/roBa_R.overlay`; the click listener's `4` in
  `config/roBa.keymap` must match `ROBA_MOUSE_LAYER` if layers are reordered.
- AML duration: both `700` values in the overlay and keymap.
- Typing guard: `require-prior-idle-ms = <200>` in the overlay.
- Mouse button positions: `excluded-positions = <18 19 20>` in the overlay.
  Update these if click bindings move.
- Scroll direction and amount: `zip_y_scaler` and the paired inertia
  `scale`/`scale-div` and `zip_scroll_scaler` values in the overlay.

The existing Shift/Z mod-tap at position 22 deactivates AML on press. Preserving
Shift+click after moving the ball would require a keymap behavior change, which
is deliberately deferred with the rest of the layer design.

Build the existing right and left matrix entries in GitHub Actions before
flashing. The local environment used for this change did not have `west`,
`cmake`, or `ninja`, so firmware compilation and hardware behavior remain
unverified.
