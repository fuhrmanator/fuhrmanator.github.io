---
layout: post
comments: true
published: true
title: SZZ (unleashed) using GitHub
header-img: img/posts/ETS-CYTech-SZZ-mashup.png
background: '/img/posts/ETS-CYTech-SZZ-mashup.png'
---

Now it's possible to use [SZZ Unleashed](https://arxiv.org/abs/1903.01742) with bug databases that are in GitHub (rather than Jira).

Thanks to excellent collaboration with [Yacine Khiter](https://www.linkedin.com/in/yacine-khiter-635501142/) (a summer intern via the [Mitacs Global Research Internship program](https://www.mitacs.ca/en/programs/globalink/globalink-research-internship)), the SZZ Unleashed (open source) implementation on GitHub was forked and has a prototype that can use GitHub as a source for bug data.

You can find the details [here](https://github.com/fuhrmanator/SZZUnleashed), including a pipeline for a small (trivial) empirical study (including some code in [Pharo](https://pharo.org/) that runs from Python) that aims to correlate the number of bugs with the size of files (for TypeScript projects).

![UML activity diagram explaining the pipeline](https://www.plantuml.com/plantuml/svg/ZP51pjem48NtFeMbRdusEK10YDw0NLV8CicG37KywSoO5fJ3TzH4I9HK_JU9FNxlqnkzWsXaBKDoquWZ9CnGZVV9_HcxkgNcEx3dagkgshffleSEjI_doJ6C4DLvNpF4rX-Phj2eL8tSjgusbMyIPJYz6UiBQDClL-D--PeLVnJurgF2cnX52eY_K6g1Ls1s28cwe1GYSxpR1lzbsi4irLKehyN3t8PpwS85Vu7ClDLK8Q7eloZoU8GdoVwb0PRTpJv8sUSod87trJNo2hsXOK2L1LrRZX2V96Ko6EOkZ17vAOevaqOr-3BnVcx8_-mnWVyOYE6bfOr7yHztaCrV_Iy-jrRiTEmZasKSg8K4OJU73kQX_InvXTaop93cPFP-Sf-FvKHfT0V8RHdIRm00){:class="img-responsive"}