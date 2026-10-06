## Notes of Mock test Udemy Course

- This process is called inference, where the model uses its trained parameters to generate a prediction or output based on new input data provided by the user
- **Inference** is the correct term for this process. It refers to the stage where a trained machine learning model is deployed to make predictions or generate
outputs based on new input data. During inference, the model uses the patterns and relationships it learned during training to provide accurate and meaningful results.
In this scenario, the user sends input data to the SageMaker model, which then performs inference to generate the corresponding output or prediction.

- **Diffusion Model** - Diffusion models create new data by iteratively making controlled random changes to an initial data sample. They start with the original data and 
add subtle changes (noise), progressively making it less similar to the original. This noise is carefully controlled to ensure the generated data remains coherent and realistic.
After adding noise over several iterations, the diffusion model reverses the process. Reverse denoising gradually removes the noise to produce a new data sample that resembles the original.

- **Generative adversarial network (GAN)** - GANs work by training two neural networks in a competitive manner. The first network, known as the generator, 
generates fake data samples by adding random noise. The second network, called the discriminator, tries to distinguish between real data and the fake data produced by the generator.
During training, the generator continually improves its ability to create realistic data while the discriminator becomes better at telling real from fake. This adversarial process 
continues until the generator produces data that is so convincing that the discriminator can't differentiate it from real data.

- **Variational autoencoders (VAE)** - VAEs use two neural networks—the encoder and the decoder. The encoder neural network maps the input data to a mean and variance for each dimension
of the latent space. It generates a random sample from a Gaussian (normal) distribution. This sample is a point in the latent space and represents a compressed, simplified version of the 
input data. The decoder neural network takes this sampled point from the latent space and reconstructs it back into data that resembles the original input.
