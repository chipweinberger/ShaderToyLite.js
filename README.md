
<p align="center">
<img src="https://github.com/chipweinberger/ShaderToyLite.js/blob/main/logo.png?raw=true" />
</p>

**ShaderToyLite.js** is a full featured ShaderToy renderer, in ~400 lines of code.

It is the rendering backend for [MidiToy](https://jamcorder.com/miditoy) ([more details](https://jamcorder.com/vfx)).

## Demo

[ShaderToyLite-demo.html](https://chipweinberger.github.io/ShaderToyLite.js/ShaderToyLite-demo.html)

^ This demo renders [Paint Streams](https://www.shadertoy.com/view/WtfyDj) by [Michael Moroz](https://michaelmoroz.github.io/Reintegration-Tracking/)

**Note:** iOS has rendering artifcacts due to `texelFetch` not being fully supported

## Features
- direcly load any ShaderToy shaders (except sound & vr) without modification
- supports buffer shaders (i.e BufferA, BufferB, BufferC, BufferD)
- pixel perfect rendering
- supports all uniforms
- upload your own textures
- shader common code (i.e. 'Common' tab in ShaderToy)
- update shaders at any time
- uses the same texture format as ShaderToy (RGBA float32)
- WebGL 2.0 (only)

**Not Supported:**
- VR, Sound, Keyboard
- pre-provided textures ('Wood', 'Rock Tiles', etc)

## Usage

```javascript
// initialize
var toy = new ShaderToyLite('myCanvas');

// add webgl textures (optional)
toy.addTexture(texture, 'rock');

// set shaders
toy.setCommon("");
toy.setBufferA({source: bufferA, iChannel0: 'rock'});
toy.setImage({source: image, iChannel0: 'A'});

// draw callback (optional)
toy.setOnDraw((){
    console.log(toy.getTime());
})

// start render loop
toy.play();

// pause render loop
toy.pause();

// currently playing?
tod.isPlaying();

// reset time to zero
toy.rewind();
```

## Minimal Example

Minimal example showing multiple buffer usage.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ShaderToyLite</title>
</head>
<body>
    <canvas id="myCanvas" width="840" height="472"></canvas>
    <script>
    var a = `
    void mainImage( out vec4 fragColor, in vec2 fragCoord ) {
        vec2 uv = fragCoord/iResolution.xy;
        vec3 col = 0.5 + 0.5*cos(iTime+uv.xyx+vec3(0,2,4));
        fragColor = vec4(col,1.0);
    }
    `;
    var image = `
    void mainImage( out vec4 fragColor, in vec2 fragCoord ) {  
        vec2 uv = fragCoord.xy / iResolution.xy;
        vec4 col = texture(iChannel0, uv);
        fragColor = vec4(col.rgb, 1.);
    }
    `;
    var toy = new ShaderToyLite('myCanvas');
    toy.setCommon('');
    toy.setBufferA({source: a});
    toy.setImage({source: image, iChannel0: 'A'});
    toy.play();
    </script>
</body>
</html>
```
