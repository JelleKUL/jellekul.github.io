# Headless Rendering

  

Because the GPU-server has no screen, you need to create a virtual one. This can be done with headless rendering.

See [moderngl headless rendering](https://moderngl.readthedocs.io/en/5.8.2/techniques/headless_ubuntu_18_server.html) for the basic overview.

  

## Setup

  

### Create virtual X-server

  

Create a new virtual sreen using `Xvfb` by defining a virtual display number, a screen and the desired resolution.

```bash

# Use the '&' to run the process in the background

Xvfb :"_displayNr_" -screen "_screenNr_" "_resolution_" &

```

Example:

```bash

Xvfb :99 -screen 0 360x360x24 &

```

  

### Define environment variable

Define it before running each script:

```bash

DISPLAY=:"_displayNr_" python "_example.py_"

```

Or define it globally for the whole session:

```bash

export DISPLAY=:"_displayNr_"

```

  

### Kill the virtual X-server process

  

When the x-server is started in the background, you need to find it again when you want to stop it.

  

List all currently running processes containing `Xvfb`

```bash

ps -ef | grep Xvfb

```

Find the process number and kill the process.

```bash

kill "_processNr_"

```

  

## Usage

  

```python

# Define a new context, define as 'standalone' optionally with the 'egl' backend

ctx = moderngl.create_context(standalone=True, backend='egl')

# Create a framebuffer

fbo = ctx.simple_framebuffer((512, 512), components=4)

fbo.use()

  

vertices = np.array([

    -1.0,  -1.0,   1.0, 0.0, 0.0,

     1.0,  -1.0,   0.0, 1.0, 0.0,

     0.0,   1.0,   0.0, 0.0, 1.0],

    dtype='f4',

)

  

prog = ctx.program(vertex_shader="""

#version 330

in vec2 in_vert;

in vec3 in_color;

out vec3 color;

void main() {

    gl_Position = vec4(in_vert, 0.0, 1.0);

    color = in_color;

}

""",

fragment_shader="""

#version 330

out vec4 fragColor;

in vec3 color;

void main() {

    fragColor = vec4(color, 1.0);

}

""",

)

vao = ctx.simple_vertex_array(prog, ctx.buffer(vertices), 'in_vert', 'in_color')

vao.render(mode=moderngl.TRIANGLES)

  

image = Image.frombytes('RGBA', (512, 512), fbo.read(components=4))

image = image.transpose(Image.FLIP_TOP_BOTTOM)

image.save('triangle.png', format='png')

```