---
layout: post
title:  "Computational complexity of ray tracing"
tags: [ Theory of Computation, Ray Tracing  ]
featured_image_thumbnail:
featured_image: 
---
![](assets/images/posts/2020/jill_raytracing.png)
*Image of a scene with ray tracing from Resident Evil 3 (2020), featured on Freemmorpg.top*

As someone who games for a hobby, I am in awe every time the world of computer graphics surpasses itself in some capacity, especially as those advancements are featured in the newest video games. However, I feel that I often take their high-quality, realistic graphics for granted. Hoping to more fully appreciate the graphics that allow me to enjoy in-game immersion, I investigated the computational complexity of a phenomenon called <strong>ray tracing</strong>, a rendering technique increasingly used in [real-time](https://www.forbes.com/sites/davealtavilla/2018/03/19/nvidia-and-microsoft-lay-foundation-for-photorealistic-gaming-with-real-time-ray-tracing/#1c057b0a6e31){:target="_blank"} for game development [1]. Dave Altavilla of <i>Forbes</i> defines the technique well:

> Ray tracing is a rendering technique that literally traces the path of light rays in an image as they illuminate a scene, bounce off objects and fill in areas around them, people or whatever is being rendered. When done right, ray tracing can render incredibly life-like imaging with much more accurate shadowing, reflections, lighting and color effects, as well as more realistic liquid, fire and smoke rendering effects that look more natural and less “game-like” to the human eye [2].

Recent iterations of ray tracing constitute one method for creating a sense of <i>photorealism</i>, that is, the result of simulating a scene such that it “[appears indistinguishable from a photograph, or by extension, from real life](https://www.tomsguide.com/us/photorealism-ten-years-why,review-1915-2.html){:target="_blank"} [3].” The main challenge facing ray tracing today is [limitations in computing power](https://blogs.nvidia.com/blog/whats-difference-between-ray-tracing-rasterization/){:target="_blank"} [4] as developers strive toward achieving greater levels of photorealism.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Zm875vmw3ng?si=ywrVlCunnjzcg9NC" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
*Comparison between scenes with and without ray tracing (RESHADE + RTX = ON means ray tracing is applied, RTX = OFF means otherwise). Credit to YouTuber Sh4down.*

More than 20 years ago, J.H. Reif, J.D. Tiger, and A. Yoshida discussed this very challenge in their paper “[Computability and Complexity of Ray Tracing](https://link.springer.com/article/10.1007/BF02574009){:target="_blank"} [5].”⁵ Reif et al. refer to the challenge as the “ray-tracing problem.” The <i>ray-tracing</i> problem is defined as the problem of tracing a single light ray through a complicated optical system with the purpose of detecting if it ever arrives at the specified destination point. Formally, they framed the problem as a [decision problem](https://xlinux.nist.gov/dads/HTML/decidableProblem.html){:target="_blank"} [6]: “Given an optical system (a finite set of reflective or refractive objects), a light ray’s initial position and direction, and some fixed point <i>p</i>, does the ray eventually reach <i>p</i>?"

They concluded that several versions of the ray-tracing problem in three dimensions are undecidable and therefore unsolvable on a Turing machine.

In particular, I focused on their first version of the ray-tracing problem: a 3D optical system consisting of a finite set of reflective objects with linear and parabolic surfaces, or refractive objects with linear and hyperbolic surfaces.

![](assets/images/posts/2020/raytracing_2.png)
*Reflection versus refraction of a light ray upon coming into contact with a surface. Figure used by Alluxa.com*

They show that ray tracing in this optical system is undecidable by simulating a universal Turing machine (TM) as an optical system. This TM has an infinite tape, and the rational number coordinates (U, V) of points like <i>p</i> in 2-D space are mapped onto the tape.

![](assets/images/posts/2020/raytracing_3.png)
*Figures 3 and 4 from Reif et al. (1994)’s paper “Computability and Complexity of Ray Tracing.” Rational coordinates of point (U, V) in 2D space are mapped onto TM’s tape.*

Reif et al. cite [Bennett](https://ieeexplore.ieee.org/document/5391327){:target="_blank"} [7] who previously demonstrated that <i>a reversible TM</i> (a TM where its transition function is 1:1, so that there is exactly one predecessor for each whole machine state) can be built to perform any possible [recursively enumerable or Turing-recognizable computation](https://www.geeksforgeeks.org/recursive-and-recursive-enumerable-languages-in-toc/){:target="_blank"} [8].

![](assets/images/posts/2020/raytracing_4.png)
*Figures 6 and 7 from Reif et al. (1994)’s paper “Computability and Complexity of Ray Tracing.” Points with rational coordinates (U, V) in 2D space are transformed into “complex boxes” in 3D space. Each complex box can represent a complex parabolic or hyperbolic surface. Each complex box also represents a finite state in a TM.*

If Reif et al. can implement complex optical boxes that can “read” the ray entering the window, perform the necessary transformations, and then redirect the ray to a complex optical box corresponding to the next state, they will have succeeded in simulating the TM. They ultimately simulate a deterministic reversible TM.

They must guarantee that the following occurs in their reversible TM: the initial transition can be invoked by projecting a ray at the position that represents the input of the TM (i.e., at the <i>entrance window</i>). A transition to the final state is simulated by the ray entering a state box designated for the final state (i.e., the <i>exit window</i>).

Again, a reversible TM has 1:1 transitions so the light ray cannot reach a point where there is an overlapping between two distinct exit windows. So Reif et al. partition the exit window of the ray into eight zones to ensure 1:1 transitions.

![](assets/images/posts/2020/raytracing_5.png)
*Figure 10 from Reif et al. (1994)’s paper “Computability and Complexity of Ray Tracing.” Exit window of the light ray is partitioned into eight zones.*

They then conclude by induction that their optical system can simulate some reversible universal TM. By showing that their model of a 3D optical system is only Turing-recognizable via a reversible TM, they then show that ray tracing in said 3D optical system is undecidable.

##### In simpler terms...
the more complicated and complex the geometry of a scene becomes, especially with respect to the various surfaces within the scene, the more difficult it becomes to implement ray tracing effects such that the scene achieves near-perfect to perfect photorealism.

Veach & Guibas (1997) at Stanford confirmed Reif et al.’s findings, stating that “[any light transport algorithm will perform very poorly as the geometry and materials of the input scene approach a provably diﬃcult conﬁguration](https://graphics.stanford.edu/papers/metro/metro.pdf){:target="_blank"} [9]."

More generally, this would mean that, due to the limitations of computing coupled with photorealistic scenes’ high computational complexity, there will be no future world in which we cannot distinguish between scenes in video games and our observable reality.

##### I believe that...
this conclusion is somewhat intuitive: as the geometric components of an input scene become increasingly complex, computing the input scene would become increasingly difficult and potentially impossible. Yet, this intuition will be tested as the field of computer graphics and ray tracing continue to evolve. One day perhaps, we may get to experience another reality other than our ours: a truly “virtual” reality. When we cross that bridge, we can certainly expect to encounter interesting questions that point to matters such as [escapism](https://pmc.ncbi.nlm.nih.gov/articles/PMC6567407/){:target="_blank"} [10] and even whether or not the reality into which we were born holds as much weight post-virtual-reality.

What still remains unclear to me is the nature of <i>reversible Turing machines</i>, a kind of universal Turing machine that Reif et al. use in their proof to show that ray tracing in their 3D optical system with a finite set of reflective or refractive objects with linear and parabolic and/or hyperbolic surfaces is undecidable. I could follow their short definition of reversible TMs, namely that these TMs have 1:1 transition functions such that that there is exactly one predecessor for each whole machine state, but I would still like a better, more in-depth understanding of what they are.

Furthermore, Reif et al. use the reversible TM to show that their 3D optical system is only recursively enumerable/Turing-recognizable and therefore undecidable. Another question that arises for me is how can the fact that their TM is Turing-recognizable guarantee its undecidability when there are also Turing-recognizable systems that are decidable?

##### To reiterate...
I examined the problem of ray tracing’s computational complexity in order to more fully appreciate the high-end graphics that bring the latest games to life. So yes, Reif et al.’s conclusion indeed informed me of the high likelihood that computer graphics with respect to ray tracing may never achieve perfect photorealism. Given this possibility, I have become more astounded at and grateful for the graphics we have today because they seem to be continuously and persistently pushing the envelope.

An arguably evident shortcoming of the findings above, nonetheless, is the era in which they were published. Reif et al.’s study on the computational complexity of ray tracing was published in 1994 while Veach & Guibas followed up a few years later in 1997. In the 1990s, video game graphics tended to be heavily pixelated and untextured.

<iframe width="560" height="315" src="https://www.youtube.com/embed/5LacCNCxOdE?si=VoZrbaapu-fidU-i" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
*Resident Evil 3: Nemesis (1999) vs. Resident Evil 3 (2020) Graphics Comparison. From IGN.*

Graphics in 2020, however, are no longer so. That being said, there could be room for improvement with respect to context. It would be fascinating to how Reif et al.’s proofs and conclusions might change (or not change) once they take into consideration the myriad achievements of modern computer graphics.

NOTE: The original Medium post can be found [HERE](https://medium.com/smith-hcv/computational-complexity-of-ray-tracing-1db936e6aba4){:target="_blank"}.

###### References

[1] Keith Stuart. (February 12 2015). Photorealism — the future of video game visuals. [https://www.theguardian.com/technology/2015/feb/12/future-of-video-gaming-visuals-nvidia-rendering](https://www.theguardian.com/technology/2015/feb/12/future-of-video-gaming-visuals-nvidia-rendering){:target="_blank"}

[2] Dave Altavilla. (March 19 2018). Nvidia and Microsoft Lay Foundation for Photorealistic Gaming with Real-Time Ray Tracing. [https://www.forbes.com/sites/davealtavilla/2018/03/19/nvidia-and-microsoft-lay-foundation-for-photorealistic-gaming-with-real-time-ray-tracing/#6e2eaa726e31] (https://www.forbes.com/sites/davealtavilla/2018/03/19/nvidia-and-microsoft-lay-foundation-for-photorealistic-gaming-with-real-time-ray-tracing/#6e2eaa726e31){:target="_blank"}

[3] Jill Scharr. (October 02 2013). The Tech Challenges to Photorealistic Games. [https://www.tomsguide.com/us/photorealism-ten-years-why,review-1915-2.html] (https://www.tomsguide.com/us/photorealism-ten-years-why,review-1915-2.html){:target="_blank"}

[4] Brian Caulfield. (March 19 2018). What’s the Difference between Ray Tracing and Rasterization? [https://blogs.nvidia.com/blog/2018/03/19/whats-difference-between-ray-tracing-rasterization/] (https://blogs.nvidia.com/blog/2018/03/19/whats-difference-between-ray-tracing-rasterization/){:target="_blank"}

[5] John H. Reif, J. Doug Tygar, and Atsumasa Yoshida. 1994. Computability and complexity of ray tracing. Discrete Comput Geom 11 (Mar. 1994), 265–288. DOI: [https://doi.org/10.1007/BF02574009](https://doi.org/10.1007/BF02574009){:target="_blank"}

[6] Paul E. Black. (December 17 2004). Dictionary of Algorithms and Data Structures. [https://www.nist.gov/dads/HTML/decidableProblem.html](https://www.nist.gov/dads/HTML/decidableProblem.html){:target="_blank"}

[7] Charles H. Bennett. 1973. Logical Reversibility of Computation. IBM J. Res. Develop. 17, 6 (Nov. 1973), 525–532. DOI: [https://doi.org/10.1147/rd.176.0525](https://doi.org/10.1147/rd.176.0525){:target="_blank"}

[8] Recursive and Recursive Enumerable Languages in TOC. [https://www.geeksforgeeks.org/recursive-and-recursive-enumerable-languages-in-toc/](https://www.geeksforgeeks.org/recursive-and-recursive-enumerable-languages-in-toc/){:target="_blank"}

[9] Eric Veach and Leonidas J. Guibas. 1997. Metropolis light transport. In Proceedings of the 24th annual conference on Computer graphics and interactive techniques (SIGGRAPH ’97). ACM Press/Addison-Wesley Publishing Co., USA, 65–76. DOI: [https://doi.org/10.1145/258734.258775](https://doi.org/10.1145/258734.258775){:target="_blank"}

[10] Kent Nordby, Ronny Andre Løkken, and Gerit Pfuhl. 2019. Playing a video game is more than mere procrastination. BMC psychology 7, 1 (Jun. 2019), 33. DOI: [https://doi.org/10.1186/s40359-019-0309-9](https://doi.org/10.1186/s40359-019-0309-9){:target="_blank"}
