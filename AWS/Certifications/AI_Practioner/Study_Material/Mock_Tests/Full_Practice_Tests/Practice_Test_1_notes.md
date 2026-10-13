## Practice Test #1 - Full-Exam - AWS Certified AI Practitioner

- **Temperature** - Temperature is a value between 0 and 1, and it regulates the creativity of the model's responses. Use a lower temperature if you want more deterministic responses,
and use a higher temperature if you want more creative or different responses for the same prompt on Amazon Bedrock.

- **Top P** - Top P represents the **percentage** of most likely candidates that the model considers for the next token. Choose a lower value to decrease the size of the pool and
limit the options to more likely outputs. Choose a higher value to increase the size of the pool and allow the model to consider less likely outputs.

- **Top K** - Top K represents the **number** of most likely candidates that the model considers for the next token. Choose a lower value to decrease the size of the pool and limit
- the options to more likely outputs. Choose a higher value to increase the size of the pool and allow the model to consider less likely outputs.

- **Stop sequences** - Stop sequences specify the sequences of characters that stop the model from generating further tokens. If the model generates a stop sequence that you specify,
it will stop generating after that sequence.

- **Amazon SageMaker Model Dashboard** : It is a centralized portal, accessible from the SageMaker console, where you can view, search, and explore all of the models in your account. You can track which models are deployed for inference and if they are used in batch transform jobs or hosted on endpoints. If you set up monitors with Amazon SageMaker Model Monitor, you can also track the performance of your models as they make real-time predictions on live data.

The Model Dashboard view extracts high-level details from each model to provide a comprehensive summary of every model in your account. If your model is deployed for inference, the dashboard helps you track the performance of your model and endpoint in real time. Within the Model Dashboard, you can select the endpoint column to view performance metrics such as CPU, GPU, disk, and memory utilization of your endpoints in real time to help you track the performance of your compute instances.

- **Amazon SageMaker Clarify** - SageMaker Clarify helps identify potential bias during data preparation without writing code. You specify input features, such as gender or age, and SageMaker Clarify runs an analysis job to detect potential bias in those features.

- **Amazon SageMaker JumpStart** - Amazon SageMaker JumpStart is a machine learning (ML) hub that can help you accelerate your ML journey. With SageMaker JumpStart, you can evaluate, compare, and select Foundation Models (FMs) quickly based on pre-defined quality and responsibility metrics to perform tasks like article summarization and image generation. Pretrained models are fully customizable for your use case with your data, and you can easily deploy them into production with the user interface or SDK.

- **Amazon SageMaker Ground Truth** - Amazon SageMaker Ground Truth offers the most comprehensive set of human-in-the-loop capabilities, allowing you to harness the power of human feedback across the ML lifecycle to improve the accuracy and relevancy of models. You can complete a variety of human-in-the-loop tasks with SageMaker Ground Truth, from data generation and annotation to model review, customization, and evaluation, either through a self-service or an AWS-managed offering.

- **Amazon SageMaker Feature Store** - Amazon SageMaker Feature Store is a fully managed, purpose-built repository to store, share, and manage features for machine learning (ML) models. Features are inputs to ML models used during training and inference. For example, in an application that recommends a music playlist, features could include song ratings, listening duration, and listener demographics.

- **Task completion rate, cost per interaction, and customer satisfaction scores**
Business objective alignment metrics measure whether an AI application achieves the outcomes that the business cares about, rather than only how well the model performs technically.
Task completion rate (or goal success rate) shows how often the agent fully resolves a customer's request, such as completing a flight change, without handing off to a human. Cost per interaction compares the cost of each automated conversation, including model tokens and tools, with the cost of a human-handled request. Customer satisfaction scores, such as CSAT, show whether customers are happy with the experience.

Together, these metrics show whether the agent improves efficiency and return on investment (ROI) while keeping customers satisfied. Amazon Bedrock AgentCore Evaluations, for example, includes a built-in goal success rate evaluator that assesses whether all user goals were completed in a session.


- **BLEU and ROUGE scores calculated against reference responses written by the human agents** - BLEU and ROUGE compare the words in generated text with reference text. They are useful for translation and summarization quality, but they do not show whether customers actually completed a flight change or refund, how much each conversation cost, or whether customers were satisfied. A response can score well on word overlap while still failing to resolve the request.

- **Token throughput, time to first token, and the number of model invocations made per day** - Token throughput, time to first token, and invocation counts are operational metrics that describe performance and usage. They help with capacity planning and cost monitoring, but they do not show whether customer requests were resolved or whether customers were satisfied. High throughput could even mean that customers need many turns to complete a single task.

- **Precision, recall, and F1 score of the intent classifier that routes customer requests** - Precision, recall, and F1 score measure how accurately a classifier assigns labels, such as customer intents. They evaluate one technical component of the system, not the business outcome. The intent classifier could be highly accurate while the agent still fails to complete flight changes, costs more than human agents, or frustrates customers.
