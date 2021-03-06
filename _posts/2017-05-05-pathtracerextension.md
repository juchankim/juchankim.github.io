---
layout: page
tags: ['Projects']
title: Extension of PathTracer
---

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <script type="text/javascript" async
    src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML">
  </script>
  <!-- <link href="/assets/finalProj/air.css" rel="stylesheet"> -->
  <style>
    .im2 {
      width: 48%;
    }
    .im3 {
      width: 30%;
    }
    .im4 {
      width: 22%;
    }
  </style>
</head>
<body>
  <h2>Subsurface Scattering</h2>
  <h4 id="by-juchan-kim-kai-sern-lim">by Juchan Kim &amp; Kai-Sern Lim</h4>
  <h2 id="abstract">Abstract</h2>

  <p>We attempted to extend the raytracer project and implement subsurface scattering in order to render translucent materials. Translucent materials do not reflect light in all directions when light hits it, some of the light goes within the material and scatters within, being emitted at other nearby locations. The Jensen paper proposes a combination of dipole model (modeling diffusion) and a single scattering model.</p>

  <img src="/assets/finalProj/bssrdf.png">

  <h2 id="technical-approach">Technical Approach</h2>
  <h3 id="dipole-model">Dipole Model</h3>

  <p>From the original (Jensen 2001) paper: the irradiance value is <br>
  <script type="math/tex" id="MathJax-Element-7786">S(x_{i},\omega_{i};x_{o},\omega_{o}) = S_{d}(x_{i},\omega_{i};x_{o},\omega_{o}) + S^{(1)}(x_{i},\omega_{i};x_{o},\omega_{o})</script>. The complete BSSRDF model is approximated by a sum of two terms, a diffusion term and a single-scattering term: <br>
  <script type="math/tex" id="MathJax-Element-7787">L(x_{o},\omega_{o})=\int_{A}\int_{\Omega}L_{i}(x_{i},\omega_{i})S(x_{i},\omega_{i};x_{o},\omega_{o})\cos(\theta)d\omega dA</script>, <script type="math/tex" id="MathJax-Element-7788">S_{d}(x_{i},\omega_{i};x_{o},\omega_{o})=\frac{1}{\pi}F_{t}(x_{i},\omega_{i})R_{d}(\|x_{i} - x_{o}\|)F_{t}(x_{o},\omega_{o})</script>. To approximate diffusion, an incoming ray is transformed into a dipole source with a real and virtual light source. <script type="math/tex" id="MathJax-Element-7789">F_{t}</script> is the Fresnel term for dielectric surfaces, <script type="math/tex" id="MathJax-Element-7790">R_{d}</script> is diffuse reflectance due to dipole source that decays exponentially as a function of <script type="math/tex" id="MathJax-Element-7791">r</script> (distance from the raytraced point).</p>

  <img src="/assets/finalProj/dipole.png">

  <p>Diffuse BSSRDF term: <script type="math/tex" id="MathJax-Element-7792">R_{d}(r)=\frac{\alpha}{4\pi}[z_{r}(\sigma_{tr}d_{r}(r)+1)\frac{e^{-\sigma_{tr}d_{tr}(r)}}{d_{r}^{3}(r)}+z_{v}(\sigma_{tr}d_{v}(r)+1)\frac{e^{-\sigma_{tr}d_{v}(r)}]}{d_{v}^{3}(r)}</script>  where <script type="math/tex" id="MathJax-Element-7793">\sigma_{tr}=\sqrt{3\sigma_{a}((1-g)\sigma_{s}+\sigma_{a})}</script>  is the effective extinction coefficient, <script type="math/tex" id="MathJax-Element-7794">d_{r}(r) = \sqrt{z_{r}^{2}+r^{2}}</script>, <script type="math/tex" id="MathJax-Element-7795">d_{v}(r)=\sqrt{z_{v}^{2}+r^{2}}</script> are real and virtual light refractions, <script type="math/tex" id="MathJax-Element-7796">z_{v}=\frac{1}{\sigma_{t}'},z_{r}=z_{v}+\frac{4}{3\sigma_{t}'}\frac{1+F_{dr}}{1-F_{dr}}</script> are real and virtual light source distances, and <script type="math/tex" id="MathJax-Element-7797">F_{dr}=-\frac{1.44}{\eta^{2}}+\frac{0.71}{\eta}+0.668+0.0636\eta</script> is the rational approximation of diffuse reflectance where <script type="math/tex" id="MathJax-Element-7798">\sigma_s</script> is the scattering coefficient, <script type="math/tex" id="MathJax-Element-7799">\sigma_t</script> is the extinction coefficient and <script type="math/tex" id="MathJax-Element-7800">\eta</script> is the index of refraction. These constants are all known properties of the material that must be chosen beforehand. The table of coefficients in the Jensen paper lists the coefficients for many materials.</p>

  <img style="width: 70%;" src="/assets/finalProj/table.png">

  <img src="/assets/finalProj/ss.png">
  <p>Single Scatter term: <script type="math/tex" id="MathJax-Element-7801">L_{o}^{(1)}(x_{o},\omega_{o}) = \frac{\sigma_{s}(x_{o})}{\sigma_{tc}}F_{t}(\eta^{-1},\omega_{o})F_{t}(\eta,\omega_{i})p(\omega_{i} \cdot \omega_{o}) e^{-s_{i}'\sigma_{t}(x_{i})}e^{-s_{o}'\sigma_{t}(x_{o})} L_{i}(x_{i}, \omega_{i})</script> where <script type="math/tex" id="MathJax-Element-7802">p</script> is the Henyey-Greenstein phase function <script type="math/tex" id="MathJax-Element-7803">p(\theta)=\frac{1}{4\pi}\frac{1-g^{2}}{[1+g^{2}-2g\cos(\theta)]^{3/2}}</script>. The <script type="math/tex" id="MathJax-Element-7804">\sigma</script> functions depend on the point on entry and the point of outgoing radiance too. Single scattering occurs when the refracted incoming and outgoing rays intersect and is computed as an integral over path lengths. <script type="math/tex" id="MathJax-Element-7805">\sigma_{t}=\sigma_{s}+\sigma_{a}</script> is the extinction coefficient and <script type="math/tex" id="MathJax-Element-7806">\sigma_{tc}=\sigma_{t}(x_{o})+\frac{|n_{i}\cdot\omega_{o}'|}{|n_{i}\cdot\omega_{i}'|}\sigma_{t}(x_{i})</script> is the combined  extinction coefficient. <script type="math/tex" id="MathJax-Element-7807">s_{o}'=\frac{\log(\xi)}{\sigma_{t}(x_{o})}</script> is the sampled distance along the refracted outgoing ray and <script type="math/tex" id="MathJax-Element-7808">s_{i}'=s_{i}\frac{\|\omega_{i}\cdot n_{i}\|}{\cos(\theta_{i}')}</script>.</p>

  <p>The formulas are all fine and dandy, but you’ll notice that these formulas all depend on the initial point of intersection and a second point nearby on the surface <script type="math/tex" id="MathJax-Element-7809">(x_{o}, x_{i})</script>. This is due to the fact that the luminance of translucent materials depends on that point and the nearby area surrounding it. To do that, we need to sample points along the surface in the nearby area around the intersect point.</p>

  <h3 id="sampling-positions">Sampling Positions</h3>

  <img src="/assets/finalProj/disk.png">

  <p>The original (Jensen 2001) paper mentioned a sampling method to generate samples in a disk around the original intersection point from the camera to the scene. This method is great for flat surfaces, but the vast majority of samples will not lie on the surface if the surface is curvy. Other sources mention augmenting this method by generating points in a disk and then shooting them in the direction of the normal vector at the original intersection point, checking for an intersection with the scene and taking those points as the points we sample.</p>

  <img src="/assets/finalProj/sphere.png">

  <img class="im2" src="/assets/finalProj/axis1.png">
  <img class="im2" src="/assets/finalProj/axis2.png">

  <p>(Kulla 2013) mentioned a novel technique where instead of just sampling on a disk, we sample on three different axis (one of which is the normal vector direction at the point of intersection) within a sphere. We bound a sphere of radius <script type="math/tex" id="MathJax-Element-7810">R_{max} = \sqrt{v/12.46}</script> and probe rays along three different axis and mark any intersection points within the sphere as the points that we draw and measure the effective radiance from.</p>

  <h3 id="importance-sampling">Importance Sampling</h3>

  <p>(Kulla 2013) mentioned that the final contribution of each point is modulated by <script type="math/tex" id="MathJax-Element-7811">\frac{R_{d}(\|P_{hit}-P\|)}{pdf_{disk}(r)}\frac{1}{|V \cdot N_{hit}|}</script> where <script type="math/tex" id="MathJax-Element-7812">pdf_{disk}(r)=R_{d}(r)/(1-e^{-R_{m}^{2}/2v})</script>, <script type="math/tex" id="MathJax-Element-7813">r(\xi) = \sqrt{-2v\log(1-\xi(1-e^{-R_{m}^{2}/2v}))}</script> and <script type="math/tex" id="MathJax-Element-7814">v = \frac{1}{2\sigma_{tr}}</script> is the formula for sampling random points on a disk. We also choose the axis to sample at a ratio of 2:1:1 for the normal vector direction versus the two other axis directions and also take that into account when modulating points by the pdf.</p>

  <h3 id="framework-changes">Framework Changes</h3>

  <p>We had to modify the <code>.dae</code> file by adding parameters, so the raytracer knew the coefficient values it needed to render the material properly. These properties were inserted XML-style into the files like:</p>

  <pre><code>&lt;technique profile="CGL"&gt; &lt;bssrdf&gt; .. &lt;/bssrdf&gt; &lt;/technique&gt;
  </code></pre>

  <p>We edited the collada file <code>collada.cpp</code> to be able to fetch the new data written into the <code>.dae</code> files:</p>

  <pre><code>if (type == "bssrdf") {
    XMLElement *e_eta = get_element(e_bsdf, "eta");
    .....
  }</code></pre>

  <p>We added a <code>BSSRDF</code> class to <code>bsdf.h/bsdf.cpp</code> to figure out which materials have that BSDF. Also added an <code>is_bssrdf()</code> variable to easily tell the class. If so, instead of estimating direct or indirect illumination, we call the <code>subsurface()</code> function in the pathtracer which adds both <code>L</code> for single scattering and diffusion.</p>

  <p>We also modified the <code>.dae</code> models with Blender, changing the light sources to point lights, removing the pedestal, and moving the lights to favorable positions.</p>

  <h3 id="workflow">Workflow</h3>

  <p>With the following methods and ideas all explained, we describe the resulting workflow of the ray tracer.</p>

  <p>Upon an intersection of a ray traced from the camera to the scene, we call the <code>subsurface()</code> method which calculates the diffusion <script type="math/tex" id="MathJax-Element-7815">S_{d}</script> and the single-scattering <script type="math/tex" id="MathJax-Element-7816">S^{(1)}</script> terms. For <script type="math/tex" id="MathJax-Element-7817">S_{d}</script> it calculates all the necessary constants and Fresnel terms <script type="math/tex" id="MathJax-Element-7818">F_{t}</script>. It generates a ray to probe into the bounding sphere of radius <script type="math/tex" id="MathJax-Element-7819">R_{max}</script> and takes the intersection of the point with the same BSSRDF material and calculates <script type="math/tex" id="MathJax-Element-7820">R_{d}</script> based on the two locations <script type="math/tex" id="MathJax-Element-7821">(x_{i},x_{o})</script>. From the area intersection point, it sends a shadow ray to sample the amount of light on that point and then we aggregate all the terms to calculate <script type="math/tex" id="MathJax-Element-7822">S_{d}</script> and divide by the pdf. For <script type="math/tex" id="MathJax-Element-7823">S^{(1)}</script>, we take the traced ray and refract it within the material and choose a random distance along this refracted ray according to exponential pdf. That will be our new sampled point, which we shoot a shadow ray in the direction of the light at which it intersects with the material again and we have our new sampled point on the surface. We have enough information to evaluate the expression for <script type="math/tex" id="MathJax-Element-7824">S^{(1)}</script>, along with all the constants and parameters. With enough samples we approximate the ground truth value of the scene.</p>

  <h3 id="problems">Problems</h3>

  <ul>
  <li><p>Most of the time was spent trying to figure out what to do. The (Jensen 2001) paper was not clear at all about what to do, and we had to consult many different resources that tried to explain what the formulas actually meant when implemented rather than some theoretical integral or implicit formulation from the paper.</p></li>
  <li><p>We spent nearly a whole week with an image that didn’t look any different than the regular diffuse case besides being very dark. We later realized that the subsurface scattering model depends on the area nearby the intersection point and we were taking light samples and shooting shadow rays from the original intersection point rather than the nearby points in the area. We also had another bug with the wrong origin point when shooting the shadow ray since we were repeating code.</p></li>
  <li><p>Since the <code>BSSRDF</code> class extended the virtual <code>BSDF</code> class from the existing raytracer and we added new methods into the <code>BSSRDF</code> class since we had a separate function for <code>subsurface()</code> scattering we thought we had to add every additional method in <code>BSSRDF</code> to the main virtual <code>BSDF</code> class and put them in the other classes that extend <code>BSDF</code> too. This was a lot of work, but we realized that since we can detect whether a BSDF is BSSRDF or not with the <code>is_bssrdf()</code> variable we included, we just left that as an <code>if</code> case and casted the BSDF into BSSRDF so we could call the additional functions. We are quite proud of this clever trick.</p>

  <p>if (isect.bsdf-&gt;is_bssrdf()) { <br>
      BSSRDF const <em>bssrdf = (BSSRDF const </em>) isect.bsdf; <br>
      bssrdf-&gt;bssrdf_methods(); <br>
      …. <br>
  }</p></li>
  <li><p>We initially had parameter issues regarding the scale of the constants (because the values listed in the table were in units of <script type="math/tex" id="MathJax-Element-7761">mm^{-1}</script> whereas the scene wasn’t) that made the material too translucent to the point that it looked like ice or glass.</p></li>
  </ul>

  <h2 id="results">Results</h2>

  <img src="/assets/finalProj/dragon.png">

  <p>This is what the dragon looks like under regular diffuse lighting.</p>

  <img src="/assets/finalProj/dragon_mis2.png">

  <p>This is what the dragon looks like when we had the issue with the scale of coefficients that made the dragon look like ice or glass.</p>

  <p>Below are the three resulting images and what the scene file looked like in Blender.</p>

  <img class="im2" src="/assets/finalProj/dragon_marble.png">
  <img class="im2" src="/assets/finalProj/dragon_scene.png">

  <p>Dragon (Marble)</p>

  <img class="im2" src="/assets/finalProj/bunny_chicken.png">
  <img class="im2" src="/assets/finalProj/bunny_scene.png">

  <p>Bunny (Chicken)</p>

  <img class="im2" src="/assets/finalProj/lucy_marble.png">
  <img class="im2" src="/assets/finalProj/lucy_scene.png">

  <p>Lucy (Marble)</p>

  <h2 id="references">References</h2>

  <p><a href="https://graphics.stanford.edu/papers/bssrdf/bssrdf.pdf">A Practical Model for Subsurface Scattering Light Transport (Jensen 2001)</a> <br>
  <a href="http://graphics.ucsd.edu/~henrik/papers/fast_bssrdf/fast_bssrdf.pdf">Fast BSSRDF (Jensen 2002)</a> <br>
  <a href="http://mgun.tistory.com/attachment/cfile9.uf@1236A13C50BFF08216A9EF.pdf">BSSRDF Summary (Koutajoki 2002)</a> <br>
  <a href="http://monet.cs.columbia.edu/tmp/herySkinBSSRDF.pdf">Implementing a Skin BSSRDF (Hery 2003)</a> <br>
  <a href="http://www.cs.cornell.edu/~ags/documents/BSSRDF_Notes.pdf">BSSRDF Notes (Scukanec 2004)</a> <br>
  <a href="http://library.imageworks.com/pdfs/imageworks-library-BSSRDF-sampling.pdf">BSSRDF Importance Sampling (Kulla 2013)</a> <br>
  <a href="http://graphics.pixar.com/library/ApproxBSSRDF/paper.pdf">Approximate BSSRDF (Christensen 2015)</a></p>



  <h2 id="contributions">Contributions</h2>

  <p><strong>Juchan</strong>: <code>.dae</code> file modification and integration with pathtracer. Coding. <br>
  <strong>Kai</strong>: Reference gathering and understanding the math and models. Coding.</p>

  <p>We coded most of the project together in the same room.</p>

  <h2 id="video-and-slides">Video and Slides</h2>

  <p><a href="https://docs.google.com/presentation/d/14yhEIX0eYCW3Cwl7MBpkXEmL3pRK8PxaBxdUE_RQAlI/edit">Slides</a> <br>
  <a href="https://youtu.be/8rluVyPwz-0">Video Explaining Slides</a></p>
</body>