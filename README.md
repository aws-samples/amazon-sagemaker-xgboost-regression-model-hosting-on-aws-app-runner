## Train a XGBoost regression model on Amazon SageMaker and host inference as an API on a Docker container running on AWS App Runner

This repository contains a sample to train a regression model in Amazon SageMaker using SageMaker's built-in XGBoost algorithm on the California Housing dataset and host the inference as an API on a Docker container running on AWS App Runner.

### Overview

[Amazon SageMaker](https://aws.amazon.com/sagemaker/) is a fully managed end-to-end Machine Learning (ML) service. With SageMaker, you have the option of using the built-in algorithms or you can bring your own algorithms and frameworks to train your models.  After training, you can deploy the models in [one of two ways](https://docs.aws.amazon.com/sagemaker/latest/dg/deploy-model.html) for inference - persistent endpoint or batch transform.

With a persistent inference endpoint, you get a fully-managed real-time HTTPS endpoint hosted on either CPU or GPU based EC2 instances.  It supports features like auto scaling, data capture, model monitoring and also provides cost-effective GPU support using [Amazon Elastic Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/ei.html).  It also supports hosting multiple models using multi-model endpoints that provide A/B testing capability.  You can monitor the endpoint using [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/).  In addition to all these, you can use [Amazon SageMaker Pipelines](https://aws.amazon.com/sagemaker/pipelines/) which provides a purpose-built, easy-to-use Continuous Integration and Continuous Delivery (CI/CD) service for Machine Learning.

There are use cases where you may want to host the ML model on a real-time inference endpoint that is cost-effective and do not require all the capabilities provided by the SageMaker persistent inference endpoint.  These may involve,
* simple models
* models whose sizes are lesser than 200 MB
* models that are invoked sparsely and do not need inference instances running all the time
* models that do not need to be re-trained and re-deployed frequently
* models that do not need GPUs for inference

In these cases, you can take the trained ML model and host it as an API on a Docker container on [AWS App Runner](https://aws.amazon.com/apprunner/).  This will be cost-effective as compared to having real-time inference instances and still provide a fully-managed and scalable solution.

[AWS App Runner](https://aws.amazon.com/apprunner/) is a fully managed service that makes it easy for developers to quickly deploy containerized web applications and APIs, at scale and with no prior infrastructure experience required.  App Runner automatically builds and deploys the web application and load balances traffic with encryption.  App Runner also scales up or down automatically to meet your traffic needs.

This example demonstrates this solution by using SageMaker's [built-in XGBoost algorithm](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) to train a regression model on the [California Housing dataset](https://www.dcc.fc.up.pt/~ltorgo/Regression/cal_housing.html).  It loads the trained model as a Python3 [pickle](https://docs.python.org/3/library/pickle.html) object in a Python3 [Flask](https://flask.palletsprojects.com/en/1.1.x/) app script in a Docker container to be hosted as an API on [AWS App Runner](https://aws.amazon.com/apprunner/).

**Warning:** The Python3 [pickle](https://docs.python.org/3/library/pickle.html) module is not secure.  Only unpickle data you trust.  Keep this in mind if you decide to get the trained ML model file from somewhere instead of building your own model.

Note:

* This notebook should only be run from within a SageMaker notebook instance as it references SageMaker native APIs.
* If you already have a trained model that can be loaded as a Python3 [pickle](https://docs.python.org/3/library/pickle.html) object, then you can skip the training step in this notebook and directly upload the model file to S3 and update the code in this notebook's cells accordingly.
* In this notebook, the ML model generated in the training step has not been tuned as that is not the intent of this demo.
* At the time of writing this notebook, [AWS App Runner](https://aws.amazon.com/apprunner/) was a new service and supported only in a few regions.  To know if this service is available in a specific region, either refer the [documentation](https://docs.aws.amazon.com/apprunner/index.html) or check in the [AWS console](https://aws.amazon.com/console/).
* At the time of writing this notebook, AWS App Runner supported only public endpoints.
* If you intend to deploy this to Production with access security, then you have to build the authentication and authorization layers in the container prior to letting the call go to the inference script.  This is not covered in this notebook.

### Repository structure

This repository contains

* [A Jupyter Notebook](https://github.com/aws-samples/amazon-sagemaker-xgboost-regression-model-hosting-on-aws-app-runner/blob/main/notebooks/sm_xgboost_ca_housing_apprunner_model_hosting.ipynb) to get started.

* [A Flask app inference script in Python3](https://github.com/aws-samples/amazon-sagemaker-xgboost-regression-model-hosting-on-aws-app-runner/blob/main/notebooks/scripts/container_sm_xgboost_ca_housing_inference.py) that contains the code for performing inference.

* [The California Housing dataset in CSV format](https://github.com/aws-samples/amazon-sagemaker-xgboost-regression-model-hosting-on-aws-app-runner/tree/main/notebooks/datasets).

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

