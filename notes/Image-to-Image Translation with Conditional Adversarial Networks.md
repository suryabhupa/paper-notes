# Image-to-Image Translation with Conditional Adversarial Networks
***Phillip Isola, Jun-Yan Zhu, Tinghui Zhou, Alexei A. Efros (2016)***

This paper strives to reduce the number of task-specific translation models needed for image transformation tasks. The authors argue that by creating a generic and powerful mapping model, many different tasks can be learned without hand-engineering specific loss or mapping functions.

The primary contributions of this work are the following:
* Successfully employ cGANs to map images of one domain to another.
* Combine conditional adversarial loss with L1 loss to show good results; cGANs learn to minimize a structured loss as opposed to the unstructured loss that CNNs minimize.
* The discriminator is patch-based, where the receptive field of the discriminator is much smaller than the image itself, allowing for generalization to larger images and better results. Moreover, this effectively models the image as a Markov random field; the "PatchGAN" can be understood as a form of texture/style loss. This also means that the PatchGAN has fewer parameters and runs faster.
* Instead of feeding in a noise vector Z, which the network learned to ignore, stochasticity was achieved by adding dropout in the generator at test and train time.
* Generator is not a simple encoder-decoder framework, but uses skip connections to bypasss bottleneck layer; dubbed "U-Net". Motivation is that low-level information shouldn't be lost.

Small details:
* Generator and discriminator are alternatingly updated one at a time.
* To evaluate "realness" of images, two metrics are used: AMT perceptual studies (using Turkers to see if images look real) and FCN-score, where pretrained nets are asked to classify synthetic images.

Open questions:
* Despite adding dropout, the level of stochasticity was quite small, and how to design cGANs to achieve higher variety in their generated images remains an open question. How can one capture "natural" colorations or symbols/logos and use these for generating more "novel" images?
* Failure cases are interesting; common failures include artifacts in regions where the input image is sparse; there was difficulty in handling unusual inputs. How do we better guard against this?
* How can cGANs be further used for semantic segmentation? Investigated preliminarily in this paper.
