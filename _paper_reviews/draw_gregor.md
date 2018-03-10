---
title: "DRAW: A Recurrent Neural Network For Image Generation"
date: 2017-11-15
authors: Karol Gregor, Ivo Danihelka, Alex Graves, Danilo Jimenez Rezende, Daan Wierstra

---

## Summary

Generative model to produce images like MNIST or StreetView House Numbers or
CIFAR (doesn't work that well)

Three major contributions:
- Uses RNNs as enc and dec in a variational autoencoder framework(which itself
  is not new)
- Multistep generation process 
- differentiable attention (has almost all the authors from previous paper-
  recurrent models of visual attention, RAM. Improved on that design which
  needed REINFORCE to train attention) 

Only advantage I can see of this is that unlike CNN, parameters don't depend on
number of pixels in the image(confirm this in eqn 25,26)

The usage of Gaussian kernels is what makes this operation differentiable-
simple matrix operations- pretty cool though (compared to RAM), it can
actually look at all the image with less detail or some of it with more.

## Things I learned

- Basics of [variational autoencoders](jaan.io/what-is-variational-autoencoder-vae-tutorial/)
    - delved a little deeper into the new generative models- GANs, autoregressive models and VAEs
- The reparametrization trick for normal distribution
    - This could have been used in Mnih's RAM model for differentiable
      attention instead of using RL
- The first loss term is common in generative models- maximize the likelihood
  of training distribution
- The latent z is drawn from a distribution that is *parametrized* by neural
  net, not predicted directly. I suppose this is to add some noise in the code
  generation so the model learns to be robust?


## Questions

- What other distributions does the reparametrization trick work on?
- Why can't simple autoencoders be used for generation?

## Answers

- http://blog.shakirm.com/2015/10/machine-learning-trick-of-the-day-4-reparameterisation-tricks/
- You can use it- just pick a code z and decode it. Problem is that random
  choices of z will lead to random outputs, which don't look like anything from
  the training distribution. So now we try to force the distribution Q(z|x) to
  look like a standard normal (referred to as prior P) which is what we
  actually sample z from during inference. **To emphasize, P is a choice we make.**
  By minimizing KL divergence between Q and P.
