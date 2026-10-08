## Notes of Mock test Udemy Course

- **Inference** is the correct term for this process. It refers to the stage where a trained machine learning model is deployed to make predictions or generate
outputs based on new input data. During inference, the model uses the patterns and relationships it learned during training to provide accurate and meaningful results.
In this scenario, the user sends input data to the SageMaker model, which then performs inference to generate the corresponding output or prediction.
The process is called inference, where the model uses its trained parameters to generate a prediction or output based on new input data provided by the user

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

- **Underfit models experience high bias, whereas, overfit models experience high variance**

- Your model is underfitting the training data when the model performs poorly on the training data. This is because the model is unable to capture the relationship between the input examples (often called X) and the target values (often called Y). Your model is overfitting your training data when you see that the model performs well on the training data but does not perform well on the evaluation data. This is because the model is memorizing the data it has seen and is unable to generalize to unseen examples.

- **Underfit models experience high bias** — they give inaccurate results for both the training data and test set. On the other hand, overfit models experience high variance - they give accurate results for the training set but not for the test set. More model training results in less bias but variance can increase. Data scientists aim to find the sweet spot between underfitting and overfitting when fitting a model. A well-fitted model can quickly establish the dominant trend for seen and unseen data sets.

- **Amazon SageMaker Canvas** gives you the ability to use machine learning to generate predictions without needing to write any code. With Canvas, you can chat with popular large language models (LLMs), access Ready-to-use models, or build a custom model trained on your data.

- **Amazon SageMaker JumpStart** : Provides one-click, end-to-end solutions for many common machine learning use cases 

- **Amazon SageMaker Clarify**: Explains how input features contribute to the model predictions during model development and inference.

- **Amazon SageMaker Data Wrangler**: The fastest and easiest way to prepare tabular and image data for machine learning.

- **Provisioned Throughput and On-Demand mode in aws bedrock**
- You can use a customized model in the Provisioned Throughput or On-Demand mode
This option is correct because Amazon Bedrock supports the use of customized models through Provisioned Throughput or On-Demand mode.
To use a custom model with dedicated compute capacity and guaranteed throughput, you can purchase Provisioned Throughput for the custom model and then use the resulting provisioned model for inference. This mode is suitable when the company needs predictable performance for steady or production-grade workloads, such as fraud detection or automated reporting pipelines that require consistent throughput. You can also deploy a custom model for on-demand inference, and after the deployment becomes active, you use the deployment ARN as the modelId for inference requests.

- **FMs use unlabeled training data sets for self-supervised learning**

- In supervised learning, you train the model with a set of input data and a corresponding set of paired labeled output data. 
Unsupervised machine learning is when you give the algorithm input data without any labeled output data. Then, on its own, the algorithm identifies patterns and relationships in and between the data. Self-supervised learning is a machine learning approach that applies unsupervised learning methods to tasks usually requiring supervised learning. Instead of using labeled datasets for guidance, self-supervised models create implicit labels from unstructured data.
Foundation models use self-supervised learning to create labels from input data. This means no one has instructed or trained the model with labeled training data sets.

- **Each AWS Region consists of a minimum of three Availability Zones (AZ)**
- **Each Availability Zone (AZ) consists of one or more discrete data centers**
- AWS has the concept of a Region, which is a physical location around the world where AWS clusters its data centers. AWS calls each group of logical data centers an Availability Zone (AZ). Each AWS Region consists of a minimum of three, isolated, and physically separate AZs within a geographic area. Each AZ has independent power, cooling, and physical security
and is connected via redundant, ultra-low-latency networks.
An Availability Zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity in an AWS Region. All AZs in an AWS Region are interconnected with high-bandwidth, low-latency networking, over fully redundant, dedicated metro fiber providing high-throughput, low-latency networking between AZs.

