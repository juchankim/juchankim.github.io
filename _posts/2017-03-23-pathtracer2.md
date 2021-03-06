---
layout: page
tags: ['Projects']
title: PathTracer2
---

<h1 align="middle">Project 3-2: PathTracer</h1>


<div>
<p> This project was an extension to the ray tracer project adding more complex stuff like the bsdf - using other materials for the objects in which we had to deal with reflection, refraction and such. Not only that, we added importance sampling rather than uniform sampling to make the convergence faster. Third part was to add in a static background (environment .exr files) and account for the lights from the environment in getting the lighting correct for the objects. Finally, we added in thin lens to create depth of field, focusing on a specific plane.  </p>

<h2 align="middle">Part 1: Mirror and Glass Materials</h2>
<p> This part was to see increase in max depth of bounces and see what the result would be on the image. When the bounce is zero, then we only account for the direct lights in which case spheres are just black because they need at least two lights to see the result - for example, reflection needs the lights from others to hit the surface of the sphere. So when the depth is 1, mirror shows the reflection while he glass one is dark because it only shows reflection and refraction is yet to hit the sphere. But you can also notice how mirror has a black sphere inside because it has yet to hit the mirror one. So the second depth shows the previous glass sphere in the reflected sphere of mirror while the glass one becomes true itself properly refracting light. One thing to note is that for depth 3, you can see light appear in the floor below the glass sphere which also lights up the surface of the glass sphere on the next depth. Fourth depth is where we can see light refracting from glass hitting the blue wall. Beyond that depth, it is almost the same until the 100th one which is the fuly converged image.    </p>
  <div align="center">
    <table style="width=100%">
      <tr>
        <td align="middle">
        <img src="/assets/proj3_2/part1/spheres0.png"  />
        <figcaption align="middle">max depth = 0</figcaption>
        </td>
        <td align="middle">
        <img src="/assets/proj3_2/part1/spheres1.png"  />
        <figcaption align="middle">max depth = 1</figcaption>
        </td>
      </tr>
      <tr>
        <td align="middle">
        <img src="/assets/proj3_2/part1/spheres2.png"  />
        <figcaption align="middle">max depth = 2</figcaption>
        </td>
        <td align="middle">
        <img src="/assets/proj3_2/part1/spheres3.png"  />
        <figcaption align="middle">max depth = 3</figcaption>
        </td>
      </tr>
      <tr>
        <td align="middle">
        <img src="/assets/proj3_2/part1/spheres4.png"  />
        <figcaption align="middle">max depth = 4</figcaption>
        </td>
        <td align="middle">
        <img src="/assets/proj3_2/part1/spheres5.png" />
        <figcaption align="middle">max depth = 5</figcaption>
        </td>
      </tr>
      <tr>
        <td align="middle">
        <img src="/assets/proj3_2/part1/spheres100.png"/>
        <figcaption align="middle">max depth = 100; "fully converged"</figcaption>
        </td>
      </tr>
    </table>
  </div>
<h2 align="middle">Part 2: Microfacet Material</h2>
<p>For lower roughness (alpha), you're expected to see more noise at the same sampling rate with high roughness. That's why there are a lot of white dots everywhere. You can see that as you go up the alpha, the surface gets really smooth and there is no noise visible.  </p>
<div align="center">
  <table style="width=100%">
    <tr>
      <td align="middle">
      <img src="/assets/proj3_2/part2/part2_dragon_au0.005.png"  />
      <figcaption align="middle">alpha = 0.005</figcaption>
      </td>
      <td align="middle">
      <img src="/assets/proj3_2/part2/part2_dragon_au0.05.png"  />
      <figcaption align="middle">alpha = 0.05</figcaption>
      </td>
    </tr>
    <tr>
      <td align="middle">
      <img src="/assets/proj3_2/part2/part2_dragon_0.25.png"  />
      <figcaption align="middle">alpha = 0.25</figcaption>
      </td>
      <td align="middle">
      <img src="/assets/proj3_2/part2/part2_dragon_0.5.png"  />
      <figcaption align="middle">alpha = 0.5</figcaption>
      </td>
    </tr>
  </table>
</div>
<p>Much more noise when the bunny is used with cosine sampling especially since it's slow at hitting the right point when it's compared at the same sampling rate since you are guaranteed to get less variance with importance sampling. </p>
<br>
  <table>
    <tr>
      <td align="left">
      <img src="/assets/proj3_2/part2/part2_bunny_cosinesampling.png"  />
      <figcaption align="middle">bunny using the default cosine sampling</figcaption>
      </td>
      <td align="right">
      <img src="/assets/proj3_2/part2/part2_bunny_mysampling.png" />
      <figcaption align="middle">bunny using the importance sampling</figcaption>
      </td>
    </tr>
  </table>

<p>I picked silicon (.614: 3.9186 0.023600; .549: 4.0903 0.041535; .466: 4.5288 0.11463) where each one is R, G, B respectively and the first value corresponds to eta and the second one is k. 
)</p>
<table>
  <tr>
    <td align="middle">
    <img src="/assets/proj3_2/part2/part2_bunny_sillicon.png"  />
    <figcaption align="middle">bunny using silicon material values</figcaption>
    </td>
  </tr>
</table>
<h2 align="middle">Part 3: Environment Light</h2>
<p>I used field.exr as my environment and the probability_debug.png that I got can be seen below. </p>
<table>
  <tr>
    <td align="middle">
    <img src="/assets/proj3_2/field.jpg" />
    <figcaption align="middle">my environment map: field converted in jpeg</figcaption>
    </td>
    <td align="middle">
    <img src="/assets/proj3_2/part3/probability_debug.png" />
    <figcaption align="middle">field probability debug.png</figcaption>
    </td>
  </tr>
  <tr>
    <td align="middle">
    <img src="/assets/proj3_2/grace.jpg" />
    <figcaption align="middle">additional environment map: grace converted in jpeg</figcaption>
    </td>
    <td align="middle">
    <img src="/assets/proj3_2/probability_debug.png"  />
    <figcaption align="middle">grace probability debug.png</figcaption>
    </td>

  </tr>
</table>

<p>Much more noise (lighter bunny: due to a lot of white dots - looks brighter) on the bunny when the bunny is used with uniform sampling especially since it's slow at hitting the right point when it's compared at the same sampling rate since you are guaranteed to get less variance with importance sampling. </p>
<br>
  <table>
    <tr>
      <td align="middle">
      <img src="/assets/proj3_2/part3/part3_bunny_unlit_importance.png" />
      <figcaption align="middle">bunny using the importance sampling</figcaption>
      </td>
      <td align="middle">
      <img src="/assets/proj3_2/part3/part3_bunny_unlit_uniform.png" />
      <figcaption align="middle">bunny using the uniform sampling</figcaption>
      </td>
    </tr>
  </table>

<p>Much more noise on the copper bunny when the bunny is used with uniform sampling especially since it's slow at hitting the right point when it's compared at the same sampling rate since you are guaranteed to get less variance with importance sampling. Same reason as the no microfacet bunny as the the stone below bunny is brighter = yet to converge. </p>
<br>
  <table>
    <tr>
      <td align="middle">
      <img src="/assets/proj3_2/part3/part3_bunny_microfacet_importance.png"  />
      <figcaption align="middle">copper bunny using the importance sampling</figcaption>
      </td>
      <td align="middle">
      <img src="/assets/proj3_2/part3/part3_bunny_microfacet_unlit_uniform.png"  />
      <figcaption align="middle">copper bunny using the uniform sampling</figcaption>
      </td>
    </tr>
  </table>
<h2 align="middle">Part 4: Depth of Field</h2>
    <table>
      <tr>
        <img src="/assets/proj3_2/part4/dragon.png" />
        <figcaption align="middle">dragon without any focus b = 0 </figcaption>
      </tr>
        <img src="/assets/proj3_2/part4/dragon0.png"  />
        <figcaption align="middle">focused on the front of dragon (b = 0.23, d= 2.3)</figcaption>
      </tr>
      <tr>
        <img src="/assets/proj3_2/part4/dragon1.png"  />
        <figcaption align="middle">focused on the back head plane of dragon (b = 0.23, d= 2.5)</figcaption>
      </tr>
      <tr>
        <img src="/assets/proj3_2/part4/dragon2.png"  />
        <figcaption align="middle">focused on the tail of dragon (b = 0.23, d= 3.0)</figcaption>
      </tr>
      <tr>
        <img src="/assets/proj3_2/part4/dragon3.png"  />
        <figcaption align="middle">focused on the back corner of the CB (b = 0.23, d= 4.0)</figcaption>
      </tr>
    </table>

<p>I attempted to focus on the tail of the dragon for different aperture sizes given in the caption below pictures. You can see that more and more become less blurry as you down the aperture size. especially distinguishably between size of 0.23 and 0.05.  </p>
    <table>
      <tr>
        <td align="middle">
        <img src="/assets/proj3_2/part4/tail0 (0.23).png"  />
        <figcaption align="middle">focused on the tail of dragon (using aperture 0.23)</figcaption>
        </td>
        <td align="middle">
        <img src="/assets/proj3_2/part4/tail1 (0.2).png" />
        <figcaption align="middle">focused on the tail of dragon (using aperture 0.2)</figcaption>
        </td>
      </tr>
      <tr>
        <td align="middle">
        <img src="/assets/proj3_2/part4/tail2 (0.1).png"  />
        <figcaption align="middle">focused on the tail of dragon (using aperture 0.1)</figcaption>
        </td>
        <td align="middle">
        <img src="/assets/proj3_2/part4/tail3 (0.05).png" />
        <figcaption align="middle">focused on the tail of dragon (using aperture 0.05)</figcaption>
        </td>
      </tr>
    </table>