A GitPitch 101 Tutorial - In 60 Seconds

To demonstrate just how simple it is to create a GitPitch slideshow 
presentation, try following along with this 60 second tutorial.

#### Step 1. Create **PITCHME.md**

Use your preferred code editor to create a file called **PITCHME.md** and add 
the following Markdown content:

```
# Flux 

An application architecture for React

#HSLIDE

### Flux Design

- Dispatcher: Manages Data Flow
- Stores: Handle State & Logic
- Views: Render Data via React

#HSLIDE

![Flux Explained](https://facebook.github.io/flux/img/flux-simple-f8-diagram-explained-1300w.png)

```

As you can see, the **PITCHME.md** content is standard Markdown. In this 
example, when GitPitch processes the content it will result in a simple 
presentation with just three slides. Note the use of the `#HSLIDE` markdown 
fragment here. It is another GitPitch convention, it simply acts as a 
[delimiter](https://github.com/gitpitch/gitpitch/wiki/Custom-Slide-Delimiters) 
to denote the separation between content on different slides in your 
presentation.

#### Step 2. Commit **PITCHME.md**

Next, simply add this file to your Git repo and push to GitLab:

```
git add PITCHME.md
git commit -m "Added my first GitPitch slideshow content."
git push
```

#### Step 3. Done!

Your GitPitch slideshow presentation is now waiting for you to share or present 
at it's public URL. Alternatively, you can 
[download](https://github.com/gitpitch/gitpitch/wiki/Slideshow-Offline) your 
slideshow for offline presentation or 
[print](https://github.com/gitpitch/gitpitch/wiki/Slideshow-Printing) it as a 
PDF document.

Note, beyond support for standard Markdown on presentation slides, GitPitch 
delivers a number of features tailored for developers, including support for 
[code blocks](https://github.com/gitpitch/gitpitch/wiki/Code-Slides), 
[GitHub GIST](https://github.com/gitpitch/gitpitch/wiki/GIST-Slides), 
[math formulas](https://github.com/gitpitch/gitpitch/wiki/Math-Notation-Slides) 
along with [image](https://github.com/gitpitch/gitpitch/wiki/Image-Slides), and 
[video](https://github.com/gitpitch/gitpitch/wiki/Video-Slides) support.
The full set of GitPitch features are documented on the 
[GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki). To see a live 
demonstration of these features 
[click here](https://gitpitch.com/gitpitch/kitchen-sink?grs=gitlab).

