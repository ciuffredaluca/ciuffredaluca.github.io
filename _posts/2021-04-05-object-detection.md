---
layout: post
title:  "[WIP] Object detection"
date:   2021-04-05 14:12:40
blurb: "A look at an example post using Bay Jekyll theme."
og_image: /assets/img/content/post-example/Banner.jpg
---

 <style>
    figure {
      border: none;
      position: inline;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
     }
  
  figcaption {
    background-color: white;
    color: black;
    font-style: italic;
    padding: 2px;
    text-align: center;
  }

  .center {
  text-align: center;
}
  </style>


# Table of Contents
1. [Part 1: Learn to localize a single object](#learn-to-localize-a-single-object)
2. [Part 2: Sliding windows](#try-to-localize-multiple-objects-sliding-windows)
3. [Footnotes](#footnotes)

# Learn to localize a single object

Assume to have a single (or none) object of interest per image in our dataset out of $$n$$ possible object classes. For instance, say we want to localize a dog in the following image:

{:.center}
![good boy](/assets/img/object-detection/labrador-retreiver.png){:width="450px"}
<br>This is a good boy.

One way to extend image classification to also localize the object is to define the following variables associated to the bounding box localizing the object:

- the *center* of the bounding box, ($$b_x$$, $$b_y$$)
- the *height* ($$b_h$$) and *width* ($$b_w$$) of the box

In addition to spatial properties we define:

- $$p_c$$ as the probability of an object to be present (either 1 or 0 in the training set)
- $$[C_1, \ldots, C_n]$$ a one-hot reresentation of the $$n$$ possible classes.[^1]

We can then finally model our target variable $$y$$ as follows:

$$
y = \begin{bmatrix}
        p_{c} \\
        b_{x} \\
        b_{y} \\
        b_{h} \\
        b_{w} \\
        C_{1} \\
        \vdots \\
        C_n
        \end{bmatrix}
$$

We assume that $$[b_{x}, b_{y}, b_{h}, b_{w} , C_{1}, \ldots, C_n]$$ are not relevant if $$p_c = 0$$ (only background is present); this is possible if the loss function has a functional form explicitly depending on $$p_c$$,

$$
L(y, \hat{y}) = \theta(y^0)f(y, \hat{y}) + (1 - \theta(y^0)) g(y^0, \hat{y}^0). 
$$

For instance, we might cast the loss as $$L_2$$ norm,

$$
L(y, \hat{y}) = \begin{cases} 
      ||y - \hat{y}||^2, & y^0 ( = p_c) = 1 \\
      (y^0 - \hat{y}^0)^2, & y^0 ( = p_c) = 0
   \end{cases}
$$

## Try to localize multiple objects: sliding windows

{:.center}
![sliding window](/assets/img/object-detection/sliding-window.gif){:width="450px"; class="img-responsive"}
<br>This is a good boy with sliding windows.

##### FOOTNOTES

[^1]: This is a note!
