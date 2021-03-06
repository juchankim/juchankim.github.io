---
layout: page
tags: ['Projects']
title: PathTracer1
---

<h1 align="middle">Project 3-1: PathTracer</h1>


<div>
<p>At a high level, this project was to create sample rays and trace it through the scene to have correct lighting and shading on the surfaces. One goes down the Bounding Volume Hierarchy, eventually ending with primitives like triangles and spheres to find the intersection.In calculating the shading even more accurately, we take into account both direct lighting (light directly to the surface) and indirect lighting (light hits the surface by another ray that got reflected by walls or other objects). We also use adaptive sampling to increase the speed of this overall process. </p>

<h2 align="middle">Part 1: Ray Generation and Intersection</h2>
<p> The basic idea in this part was to create camera rays that get sampled (and averaged per pixel) that will trace throughout the scenes. So the first part is to create the ray. Given an image plane with all the pixels that is one unit away from the original point ("pinhole/camera"), we displace it to fit within that plane. And then, get it back to the world coordinates for the ray to be actually created. Such rays will get called with trace_ray to trace throughout the scene and eventually return color that will get averaged by the number of samples taken for that pixel.
The next part was intersecting primitives like triangles and spheres. I chose Moller Trumbore algorithm to check for intersections that takes advantage of barycentric coordinates of a point within a triangle [formula given below]. If no value, then we return false. However, getting a value doesn't automatically mean that that's the correct time since the intersection point can be out of the supported range by the ray.  </p>
<div align="middle">
  <table style="width=100%">
      <tr>
          <img src="/assets/proj3_1/part1_alg.png" />
          <figcaption align="middle">Moller Trumbore algorithm</figcaption>
      </tr>
      <tr>
          <img src="/assets/proj3_1/part1_bench.png" />
          <figcaption align="middle">bench with normal shading</figcaption>
      </tr>
      <tr>
          <img src="/assets/proj3_1/part1_bunny.png"  />
          <figcaption align="middle">bunny with normal shading</figcaption>
      </tr>
      <tr>
          <img src="/assets/proj3_1/part1_CBspheres_lambertian.png" />
          <figcaption align="middle">spheres in a box with normal shading</figcaption>
      </tr>
  </table>
</div>
<h2 align="middle">Part 2: Bounding Volume Hierarchy</h2>
<p> The basic idea of this part was to Bouncing Volume Hierarchy (BVH), a structure that partitions primitives into two groups to check fewer primitives at the end (when we finally recurse down to the leaf node that contains minimum number of primitives). How to construct the structure? find a big bounding box by extending the bounds of the given primitives of that node. Then, if it's greater than the minimum number of nodes, then split into two child nodes. See which dimension is the longest and split on the midpoint of the longest dimension for division of primitives. After doing so, recurse through left and right nodes until the base case of a leaf node. </br></br> The intersection algorithm was first checking the box of the node. If it's doesn't hit the box at all, it doesn't intersect whichever ones are inside. Then, see if it's a leaf node, then the only way is to go through all the primitives and see which one gets hit first. Else, we check on both left and right child nodes and see which one has the closest point.  </p>
<div align="center">
    <table style="width=100%">
        <tr>
            <img src="/assets/proj3_1/part2_blob.png" />
            <figcaption align="middle">blob with normal shading</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part2_CBlucy.png"  />
            <figcaption align="middle">lucy in the box with normal shading</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part2_maxplanck.png"  />
            <figcaption align="middle">maxplanck with normal shading</figcaption>
        </tr>
    </table>
</div>
<h2 align="middle">Part 3: Direct Illumination </h2>
<p>  As mentioned, we are tracing back to the light source. So we loop through the given scene lights to see if the radiance can count for the hit point. For the direct lighting estimation, it should directly hit the surface instead of hitting any other surfaces. We skip if it does not satisfy the values of going beyond lights for an object. Else, add the light's radiance value into the final radiance value. However, it is important to note to divide each term by number of lights and number of samples so that we can just add instead of worrying about the division and maybe even going over the limit for the primitive values. </br> </br> Looking at the photos below, it is easy to note the loss of noise as we provide more light rays available, creating a smoother surface overall including the shadows. However, noise still exists. If we rendered at even higher number, it might be even smoother and natural looking instead of noise.   </p>
<div align="center">
    <table style="width=100%">
        <tr>
            <img src="/assets/proj3_1/part3_bench.png" />
            <figcaption align="middle">bench with direct illumination </figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part3_dragon_64_32.png"  />
            <figcaption align="middle">dragon with direct illumination </figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part3_CB_4.png"  />
            <figcaption align="middle">CBbunny with direct illumination (l=1,s=1)</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part3_CB_4.png" />
            <figcaption align="middle">CBbunny with direct illumination (l=4,s=1)</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part3_CB_16.png"  />
            <figcaption align="middle">CBbunny with direct illumination (l=16,s=1)</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part3_CB_64.png"  />
            <figcaption align="middle">CBbunny with direct illumination (l=64,s=1)</figcaption>
        </tr>
    </table>
</div>
<h2 align="middle">Part 4: Indirect Illumination </h2>
<p> The basic idea of this part was to bring out lights that are hitting the surface indirectly. First determine which ray to traverse by sampling from bsdf (that has the incoming radiances). We call trace_ray recursively with decreased depth so that it will eventualy stop when it goes to 0 OR it stops due to termination probability that gets determined by the coin flip. If we don't terminate, then generate a ray and gets its incoming radiance to convert it to outgoing radiance by multiplying L by the bsdf * cos_theta(w_in) and dividing by pdf and termination probability. This will estimate the indirect lighting.  </p>
<div align="center">
    <table style="width=100%">
        <tr>
            <img src="/assets/proj3_1/part4_blob(global).png"  />
            <figcaption align="middle">blob with global illumination</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_spheres(global).png" />
            <figcaption align="middle">spheres in a box with global illumination</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_spheres_direct.png"  />
            <figcaption align="middle">spheres in a box with only direct illumination</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_spheres_indirect.png" />
            <figcaption align="middle">spheres in a box with only indirect illumination</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_bunny_max0.png"  />
            <figcaption align="middle">bunny with max_rax_depth=0</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_bunny_max1.png" />
            <figcaption align="middle">bunny with max_rax_depth=1</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_bunny_max2.png"  />
            <figcaption align="middle">bunny with max_rax_depth=2</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_bunny_max3.png"  />
            <figcaption align="middle">bunny with max_rax_depth=3</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_bunny_max100.png"  />
            <figcaption align="middle">bunny with max_rax_depth=100</figcaption>
        </tr>
        <tr>

            <img src="/assets/proj3_1/part4_sphere_spp1.png" />
            <figcaption align="middle">spheres with sample/pixel=1</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_sphere_spp2.png"  />
            <figcaption align="middle">spheres with sample/pixel=2</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_sphere_spp4.png"/>
            <figcaption align="middle">spheres with sample/pixel=4</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_sphere_spp8.png" />
            <figcaption align="middle">spheres with sample/pixel=8</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_sphere_spp16.png" />
            <figcaption align="middle">spheres with sample/pixel=16</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_sphere_spp64.png" />
            <figcaption align="middle">spheres with sample/pixel=64</figcaption>
        </tr>
        <tr>
            <img src="/assets/proj3_1/part4_sphere_spp1024.png" />
            <figcaption align="middle">spheres with sample/pixel=1024; hard to see noise </figcaption>
        </tr>
    </table>
</div>
<h2 align="middle">Part 5: Adaptive Sampling </h2>
<p> The basic idea in this part was to avoid using unnecessary number of samples per pixel if there is no need to. So if we can see that there is convergence in the value of I (1.96 * sqrt(variance/num_samples_so_far) being smaller than maxTolerance * mean), then we stop there. Else, we keep going until the max number of samples for each pixel. And to avoid even more cost, only calculate and check this value only for each Batch samples.  </p>
<div align="center">
    <table style="width=100%">
        <tr>
            <img src="/assets/proj3_1/part5_bunny_rate.png"/>
            <figcaption align="middle">bunny with max num of samples = 2048; rate difference is clear with red and green in stark contrast</figcaption>
        </tr>
    </table>
</div>
</div>