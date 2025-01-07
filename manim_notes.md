# Manim Fundamentals:
---

- The most Basic class is Scene class, which must be inherited for our scene, this class must also impliment the construct methdo where all the discription lives.

- the play method of the class is responsible of putting objects on screen, it takes one or more Animation like Write, FadeOut, FadeIn etc..., it also takes run_time parameter which takes duration of animation which defaults to 1 second.

- To render the video, we will use the **manim file_name -pqm** command, where

- **p** is the preview flag, telling Manim that we want to immediately view the result and
- **qm** sets the quality to **m**edium (others being **l**ow, **h**igh and 4**k**),

- Go [here](https://slama.dev/manim/introduction/) for more info.

- In the code the sequance of self.play statements matters as well as sequance of grouping objects via vgroup statements also matters. 
