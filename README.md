# Raytracer

Welcome to my **Raytracer Project**&mdash;an exploration into the world of computer graphics, built from the ground up in C++! Using the [_Ray Tracing in One Weekend_](https://raytracing.github.io/books/RayTracingInOneWeekend.html) series as a starting point, I'm excited to expand upon the foundational concepts and push the boundaries with new features and optimizations as I learn more. Follow along as I delve deeper into the math, coding, and art of ray tracing!

<img src="/example_images/image01.png" width=100%>

## Current Features

This project currently supports a variety of features for simulating realistic materials, lighting, and camera effects. Below is a breakdown of the key components and their functionality, which form the foundation of the raytracing engine:

### Materials
- **Metal** &mdash; Supports realistic reflections, with adjustable blur for glossy or mirror-like surfaces.
- **Diffuse** &mdash; Implemented Lambertian reflection for more accurate light diffusion across matte surfaces.
- **Dielectric** &mdash; Simulates glass-like materials with refraction, total internal reflection, and Fresnel effects using Snell's Law and Schlick's Approximation.

### Camera
- **Positioning** &mdash; Flexible camera positioning using `lookat` and `lookfrom` parameters for intuitive scene setup.
- **Depth of Field** &mdash; Achieve realistic focus effects with a defocus disk and focus distance for controlling depth of field.
- **Sample Size** &mdash; Customize image quality by specifying the number of samples used per pixel for smoother, less noisy results.

### Geometry
- **Spheres** &mdash; Support for spheres with adjustable radii, and more complex shapes coming soon.

## How to Use
### Initializing the Scene
Start by creating a `hittable_list` object to hold all the geometry in your scene. This should be initialized at the beginning of your `main` file:
```
int main() {
  hittable_list world;
  ...
}
```

### Adding Materials
Next, define the materials for your scene. Currently, the following materials are supported:
1. **Lambertian (Diffuse)**: For matte/diffuse materials, uses the Lambertian reflectance model. \
Create a Lambertian material by initializing a shared pointer to it. The constructor takes a `color` object representing the material's color: 
```
// Create a lambertian material with a grey color
auto lambertian_mat = make_shared<lambertian>(color(0.5, 0.5, 0.5));
```
2.  **Metal**: For reflective materials. Metal will reflect all light rays. \
Create a metal material by initializing a shared pointer to it. The `metal` constructor takes two parameters: the `color` of the material and a `double` between 0 and 1 representing the reflection blur (fuzz):
```
// Create a metal material with a grey color and fuzzy reflections
auto metal_mat = make_shared<metal>(color(0.5, 0.5, 0.5), 0.8);
``` 
3. **Dialectric**: For refractive materials like glass or water.  \
Create a dialectric material by initializing a shared pointer to it. The `dialectric` constructor takes one parameter: the index of refraction (`double`) of the material.
```
// Create a  dialectric material with an IOR of 1.33 (same as water)
auto dialectric_mat = make_shared<dialectric>(1.33);
``` 

### Adding Geometry
Once you have defined your materials, you can add geometry to your scene. Currently, only spheres are supported (more shapes coming very soon!). To add a sphere, initialize a shared point to a `sphere` object and add it to the `hittable_list`. 
```
// Add a sphere to the world using material mat
world.add(make_shared<sphere>(point3(0, 0, -1), 0.5, mat);
```
The `sphere` constructor takes three parameters: 
- `center`: A `point3` object for the sphere's center coordinates
- `radius`: A `double` representing the sphere's radius
- `mat`: A shared pointer to the sphere's material.


### Camera Controls
After adding geometry, set up the camera:
```
// Initialize camera
camera cam;
```
You can configure the following camera properties:
- `aspect_ratio` (`double`): Ratio of image width over image height (default=1.0)
- `image_width` (`int`): Image width in pixels (default=100)
- `samples_per_pixel` (`int`): Count of random samples per pixel (default=10)
- `max_depth` (`int`): Maximum number of times a ray will bounce in the scene (default=10)
- `vfov` (`double`): Vertical field of view in degrees, used for zooming in and out (default=90)
- `lookfrom` (`point3`) Position of the camera (default=(0, 0, 0,))
- `lookat` (`point3`): Point the camera is looking at (default=(0, 0, -1))
- `vup` (`vec3`) Camera-relative 'up' direction (default=(0,1,0))
- `defocus_angle` (`double`): Variation angle in degrees of rays through each pixel, used for setting depth of field (default=0.0)
- `focus_distance` (`double`): Distance from the camera `lookfrom` point to the plane of perfect focus (default=10).

You can set any of these parameters directly. For example,
```
// Set the aspect ratio to 16:9
cam.aspect_ratio = 16.0 / 9.0;
```

### Rendering
Finally, render the scene using the camera:
```
// Render the scene
cam.render(world);
```
Compile and run the program, saving the output to a `.ppm` file.
```
g++ -o main main.cpp
./main > image.ppm
```
Your rendered image will be saved as `image.ppm`.

## Roadmap: What's Next?

Here's a sneak peek into what I have planned for the future:

- **Texture Mapping** — Integrating image textures with UV mapping to give surfaces more detail and realism.
- **Multithreading** — Speeding up render times by utilizing multi-core processing for faster, more efficient rendering.
- **Physically Based Rendering (PBR)** — Elevating the lighting model to be more physically accurate for more photorealistic results.
- **Triangle Meshes** — Expanding the range of supported shapes by introducing triangle meshes for complex geometry.


