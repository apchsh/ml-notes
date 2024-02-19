## Notes on the subject of text 2 video models

OpenAI Sora Technical report: https://openai.com/research/video-generation-models-as-world-simulators

Summary: 
- SORA uses 'patches' (produced by a visual encoder) and a transformer-diffuser model to train up to 60 second video clips of
  various different apsect ratios and resolutions
- The 'spacetime' patches act as tokens for the transformer / and represent a compressed low-dim latent space
- The diffusion transformer scales just as well with compute as transformers do for LLMs
- SORA appears to be capturing real world properties like object permanence, interactions and some physics however this is with limitations

### Lit Review  

