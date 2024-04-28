from transformers import AutoModelForCausalLM, AutoTokenizer
import torch

def setup_model(model_name):
    # Load model and tokenizer from Hugging Face
    tokenizer = AutoTokenizer.from_pretrained(model_name)
    model = AutoModelForCausalLM.from_pretrained(model_name)
    return tokenizer, model

def generate_chat_completion(tokenizer, model, prompt, max_length=50):
    # Encode the prompt into tokens
    input_ids = tokenizer.encode(prompt, return_tensors='pt')

    # Generate completion using the model
    output_ids = model.generate(input_ids, max_length=max_length)

    # Decode the generated tokens to a string
    completion = tokenizer.decode(output_ids[0], skip_special_tokens=True)
    return completion

# Model name - replace 'gpt2' with 'facebook/llama-13b' if available
model_name = 'gpt2'
tokenizer, model = setup_model(model_name)

# Generate a completion
prompt = "Hello, how can I help you today?"
completion = generate_chat_completion(tokenizer, model, prompt)
print(completion)
