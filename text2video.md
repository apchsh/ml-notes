## Notes on the subject of text 2 video models and OpenAI/SORA

OpenAI Sora Technical report: https://openai.com/research/video-generation-models-as-world-simulators

**Summary** 
- SORA uses 'patches' (produced by a visual encoder) and a transformer-diffuser model to train up to 60 second video clips of
  various different apsect ratios and resolutions
- The 'spacetime' patches act as tokens for the transformer / and represent a low-dim latent space
- The diffusion transformer scales well with compute (similarly as transformers do for LLMs)
- SORA appears to be capturing real world properties like object permanence, interactions and some physics however this is with limitations

### Lit Review  

The report points out a few different architectures which have been used for generative modelling of video data, not limited to RNNs, GANs, autoregressive transformers and diffusion models. The report states that these prior approaches were often limited in video size, duration or contents. 

#### Recurrent Networks:

Three references were included: Srivastava (2015), Chiappa (2017) and Ha (2018)

Links: 
- Srivastava (2015) - https://arxiv.org/abs/1502.04681

**Srivastava (2015) - Hereafter SR15:** 
  - LSTMs were in vogue at this point for problems which required modelling a sequence
  - Sutskever (2014) proposes a general sequence to sequence architecture which uses an encoder which produces a fixed length representation followed by a  decoder
  - Makes the point that video is a window into the physics of the real-world. Learning good representations is therefore important
  - They point out that supervised learning approaches for video (3D conv nets, temporal fusion strategies (?), different video representations for convnets) suffer from dimensionlaity problems which are not easy to overcome e.g. capturing long-range dependancy, credit assignment, not enough labelled data etc...
  -  The inductive bias of the LSTM architecture is that the same operation is applied at each time step therefore 'physics' of the world remains same (is this actually the case I wonder? Will the physics learned by the operator apply uniformly?)
  -  Prior work for unsup video generation used ICA, generative models for mapping between images and RNNs. The latter, Ranzato el al. (2014) point out the difficulty of designing a loss function which works for video gen since squared loss does not respond well to inpur space. They use a dictionary of patches instead. SR15 argue that designing such a loss function is a non-starter and better to focus on designing network architecture which works with squared loss

Architecture:
  - Two decoders are used one which predicts the current image (autoencoder) and one which predicts the future image (predictor)
  - Two inputs are used, (natural?) 'image patches' from a dataset of moving MNIST digits and high-level percepts (states of last / 2nd-2-last layer of ImageNet)
  - Standard LSTMs as far as I can tell
  - Conditional decoder receives last generated output frame as input
  - SR15 argue that conditioning the decoder may work better for predictor since target distribution is not unimodal, but worse for optimisation since frame to frame only very small changes happen in video
  - SR15 make a composite model by running both tasks simultaneously 

Evaluation:
  - Look at reconstruction and predictions of the model w.r.t to video sequence
  - Quantitative eval on supervised action recognition dataset
