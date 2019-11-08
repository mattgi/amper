# amper

Another Sunday, another early morning coffee and some coding. But today things are at a point where writing about the last few weekends is worthwhile. A README update is necessary!

## Overview

This repository scratches an itch.

I've been wanting to explore AMP and see what is possible and how one may build a design tool around it. [@kodemon](https://github.com/kodemon) has had ideas he also wanted to explore, so the current state is a result of ~40 hours of pair programming, mostly very late on Saturday nights when we should be living our best lives (or, better still, sleeping).

Where have we got to? We now have a collection of mostly-working ideas. Code is not written with production or even professional usage in mind - so arm yourself first with caffeine or alcohol if you plan to sift through it.

<p align="center">
<img src="https://github.com/mattgi/amper/blob/master/doc/media/lp.gif" />
</p>

### Motivation

I've been keen on AMP since it was released. Page engines are fun to build but browser nuances make them frustrating to maintain - they are actual work! This repository laughs in the face of the daily grind, it's just for fun - if we want things to only run when you're hopping on one leg and your wifi is dropping out, we will make it so!

With that mindset, what if one could just leverage AMP and focus more on a slick UX for generating sites? Heck, what if you could use a web app on an iPad to build a site and then spit out AMP... but also, as a stretch, print it as a photobook? (the bar is pretty low on good design in that space, a lot of legacy flash and weird desktop publishing apps).

## Ideation and Iteration

### MVP

Some key goals:
- a React app with 3 basic views - welcome, dashboard, and designer,
- serverless, ideally offline first/capable,
- realtime, live previews, and collaborative, and
- maybe add GitHub Auth and Netlify hosting if things get that far.

![](https://github.com/mattgi/amper/blob/master/doc/media/mvp-flow.png)

### Requirements

#### Collaborative Text Editing

Collaborative text editing with some CRDT-friendly editor is a hard requirement.. but don't want to reinvent the wheel on this, use Quill (or similar). Once collaborative text editing and live previewing is working, anything should be possible so lets try to add new components collaboratively as well.

#### Layers and Grids

Things need to live on top of each other.

<p align="center">
<img src="https://github.com/mattgi/amper/blob/master/doc/media/layerstack.png" />
</p>

Each chapter (or section) has a layer stack. Each layer on the stack is a CSS Grid layout that can have components added as needed. Components can also manage internal layouts (e.g. text columns).

![](https://github.com/mattgi/amper/blob/master/doc/media/grid.png)
![](https://github.com/mattgi/amper/blob/master/doc/media/columns.png)

#### Images

1. Use something like Imgix or Cloudinary (or Dropbox) for media uploads. Keep it serverless. Also use an Asset Manager or Media Library when uploading images. This can be basic for an MVP - upload to one location, copy paste the URL for usage in a page.

2. Find a way to inset or allow images to have borders.
_Using a grid layout and adding some padding makes this trivial_.

<p align="center">
<img width="400" src="https://github.com/mattgi/amper/blob/master/doc/media/img.png" />
</p>

3. Allow overlays to sit above or below (underlays) images.. put them on their own layer and let them utilise their grid - have 10 if you want.

<p align="center">
<img src="https://github.com/mattgi/amper/blob/master/doc/media/grid+overlays.png" />
</p>

#### Editor and Settings

The canvas should run the rendering engine (which incidentally should be vanilla javascript feeding in to AMP). Any editor interaction with the page should occur by React overlaying or injecting itself (portals) where it makes sense (but hopefully just sitting to the side in settings panels (or maybe floating?)).

<p align="center">
<img width="400" src="https://github.com/mattgi/amper/blob/master/doc/media/settings-pane-float.png" />
</p>

#### Functional and Simple

Get all the settings in place first, but then pair it back so anyone can feel productive and push out something that looks decent. Intuitive presets, but an underlying architecture to let advanced users do anything. Start with _information overload_ so its easy in development to tweak the knobs.. then remove the knobs - function over form to start!

##### Functional:

<p align="center">
<img src="https://github.com/mattgi/amper/blob/master/doc/media/functional.png" />
</p>

##### Simpler, is it simple enough? No.

<p align="center">
<img src="https://github.com/mattgi/amper/blob/master/doc/media/nav-simple.png" />
</p>

### Beyond an MVP

Most things work but there has not been much focus on AMP.. it's not a valid AMP page! Hard fail! Then again, without a server in the mix, this is actually technically impossible as it needs to be statically generated so you can't beat yourself up too much ;) Some lambda in Netlify can take care of this... and I did start on the Github/Netflify integration but this is definitely beyond the scope of _scratching an itch_ so probably something not to be explored further. 

## Wrapping up

To recap, it's been an adventure. Working (mostly) is:
- a structured approach to rendering:
  - sites have pages,
  - pages have sections,
  - sections have layer stacks,
  - layers are CSS Grids and house components.. 
  - components can be anything
- a vanilla JS engine that receives this structured JSON and renders HTML/AMP. It is also reactive so re-renders when the JSON changes, and emits events to anyone else listening so is used in the designer, in live preview, and, ultimately in the final rendered page.
- a ReactJS designer that supports collaborative editing, live previews, asset management, responsive breakpoints, and template building.
- a concept that sites are published, not pages
- finally, it's all without a backend - local storage and peerjs ftw!

##  Disclaimer

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

This software was designed to be used only for research purposes.
Production use is not recommended, and should not be attempted.
This software comes with no warranties of any kind whatsoever, and may not be useful for anything. Use it at your own risk!

If these terms are not acceptable, you aren't allowed to use the code.