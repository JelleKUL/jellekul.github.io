
# Open3D Controls

## Mouse view control
  Left button + drag         : Rotate.\
  Ctrl + left button + drag  : Translate.\
  Wheel button + drag        : Translate.\
  Shift + left button + drag : Roll.\
  Wheel                      : Zoom in/out.

## Keyboard view control
  [/]          : Increase/decrease field of view.\
  R            : Reset view point.\
  Ctrl/Cmd + C : Copy current view status into the clipboard.\
  Ctrl/Cmd + V : Paste view status from clipboard.

## General control
  Q, Esc       : Exit window.\
  H            : Print help message.\
  P, PrtScn    : Take a screen capture.\
  D            : Take a depth capture.\
  O            : Take a capture of current rendering settings.\
  Alt + Enter  : Toggle between full screen and windowed mode.

## Render mode control
  L            : Turn on/off lighting.\
  +/-          : Increase/decrease point size.\
  Ctrl + +/-   : Increase/decrease width of `geometry.LineSet()`.\
  N            : Turn on/off point cloud normal rendering.\
  S            : Toggle between mesh flat shading and smooth shading.\
  W            : Turn on/off mesh wireframe.\
  B            : Turn on/off back face rendering.\
  I            : Turn on/off image zoom in interpolation.\
  T            : Toggle among image render: *no stretch / keep ratio / freely*stretch.

## Color control
  0..4,9       : Set point cloud color option.\
                 0 - Default behavior, render point color.\
                 1 - Render point color.\
                 2 - x coordinate as color.\
                 3 - y coordinate as color.\
                 4 - z coordinate as color.\
                 9 - normal as color.\
  Ctrl + 0..4,9: Set mesh color option.\
                 0 - Default behavior, render uniform gray color.\
                 1 - Render point color.\
                 2 - x coordinate as color.\
                 3 - y coordinate as color.\
                 4 - z coordinate as color.\
                 9 - normal as color.\
  Shift + 0..4 : Color map options.\
                 0 - Gray scale color.\
                 1 - JET color map.\
                 2 - SUMMER color map.\
                 3 - WINTER color map.\
                 4 - HOT color map.