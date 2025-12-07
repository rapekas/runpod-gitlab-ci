# runpod-gitlab-ci
GitLab CI/CD configuration to automate the deployment, management, and teardown of GPU-powered pods on [RunPod.io](https://runpod.io) for running Large Language Models with vLLM

## Features

*   **Automated Deployment:** Creates RunPod GPU pods via the RunPod API based on configurable parameters.
*   **LLM Ready:** Configures pods to run LLMs using the vLLM framework.
*   **CI/CD Stages:** Includes stages for deploying, listing, stopping, starting, and destroying pods directly from GitLab CI/CD.
*   **Configuration via Variables:** Uses GitLab CI/CD variables for easy customization of pod specifications (GPU type, model, disk space, etc.) and secrets (API keys, tokens).
*   **Information Output:** Provides detailed information about the created pod (cost, location, specs) during deployment.

## Prerequisites

1.  **GitLab Account & Project:** You need a GitLab instance and a project to host this `.gitlab-ci.yml` file.
2.  **RunPod Account:** An active account on [RunPod.io](https://runpod.io).
3.  **RunPod API Key:** A full-rights API key from your RunPod account.
4.  **Hugging Face Token:** A token from [Hugging Face](https://huggingface.co/settings/tokens) (required if using gated LLM models).
5.  **SSH Keys (Optional):** For direct SSH access to the pod, if needed.

## Setup

1.  **Add Variables to GitLab CI/CD:**
    In your GitLab project, navigate to `Settings` -> `CI/CD` -> `Variables`.
    Add the following variables:
    *   `RUNPOD_API_KEY`: Your RunPod API key (Masked and Protected recommended).
    *   `HG_TOKEN`: Your Hugging Face token (Masked and Protected recommended).
    *   `RUNPOD_SSH_PUBLIC_KEYS`: Your public SSH keys for pod access (if applicable, separated by `\n` if multiple).
    *   `RUNPOD_SSH_PRIVATE_KEY`: Your private SSH key for GitLab Runner to connect to the pod (Masked and Protected recommended).

2.  **Configure `.gitlab-ci.yml`:**
    Review the `variables` section in the provided `.gitlab-ci.yml` file. Adjust default values like `HG_LLM`, `RUNPOD_GPU_TYPE`, `RUNPOD_TEMPLATE`, etc., as needed for your use case. Ensure the specified template (`RUNPOD_TEMPLATE`) or image (`RUNPOD_IMAGE`) supports your target LLM and GPU requirements.

3.  **Runner Tags:**
    Ensure your GitLab Runner has the tags `gpu` and `runpod` (or whatever tags you specify in the `.gitlab-ci.yml`) to pick up these jobs.

## Usage

1.  **Deploy a Pod:** Manually trigger the `deploy_pod` job in your GitLab CI/CD pipeline. Monitor the logs for pod creation details and the RunPod console for deployment status.
2.  **List Pods:** Manually trigger the `list_all_pods` job to see the status and details of existing pods.
3.  **Stop/Start:** Manually trigger the `stop_pod` or `start_pod` jobs as needed.
4.  **Destroy a Pod:** Manually trigger the `destroy_pod` job to terminate the pod and stop incurring charges. This is crucial for cost management.

## Important Notes

*   **Costs:** Running GPU pods incurs costs on RunPod.io. Always destroy pods when not in use.
*   **Manual Jobs:** All stages are set to `when: manual` for explicit control. Trigger them as needed.
*   **Gated Models:** Ensure you have access to the specified Hugging Face model and have set the `HG_TOKEN` variable if the model is gated.
*   **Security:** Store sensitive information like API keys and tokens securely using GitLab CI/CD variables.

## License

This project is licensed under the MIT License.
