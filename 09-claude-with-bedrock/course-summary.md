# Claude in Amazon Bedrock - Course Summary

**Course URL:** https://anthropic.skilljar.com/claude-in-amazon-bedrock

---

## 🎯 Course Overview

Deploy and use Claude models via Amazon Bedrock. Covers API access, model selection, prompt engineering, tool use, RAG implementations, streaming, and building AI workflows using AWS infrastructure.

---

## 📚 Table of Contents


**Course introduction**
  📌 Introduction to the course
  📌 Overview of Claude Models

**Working with the API**
  📌 Accessing the API
  📌 Making a request
  📌 Multi-Turn conversations
  📌 Chat bot exercise
  📌 System prompts
  📌 System prompt exercise
  📌 Temperature
  📌 Streaming
  📌 Controlling model output
  📌 Structured data
  📌 Structured data exercise
  ✅ Quiz on working with the API

**Prompt evaluations**
  📌 Prompt evaluation
  📌 A typical eval workflow
  📌 Generating test datasets
  📌 Running the eval
  📌 Model based grading
  📌 Code based grading
  📌 Exercise on prompt evals
  ✅ Quiz on prompt evaluations

**Prompt engineering**
  📌 Prompt engineering
  📌 Being clear and direct
  📌 Being specific
  📌 Structure with XML tags
  📌 Providing examples
  📌 Exercise on prompting
  ✅ Quiz on prompt engineering

**Tool use**
  📌 Introducing tool use
  📌 Tool functions
  📌 JSON Schema for tools
  📌 Handling tool use responses
  📌 Running tool functions
  📌 Sending tool results
  📌 Multi-Turn conversations with tools
  📌 Adding multiple tools
  📌 Batch tool use
  📌 Structured data with tools
  📌 Flexible tool extraction
  📌 The text editor tool
  ✅ Quiz on tool use

**Retrieval Augmented Generation**
  📌 Introducing Retrieval Augmented Generation
  📌 Text embeddings
  📌 The full RAG flow
  ✅ Quiz on Retrieval Augmented Generation

**Features of Claude**
  📌 Extended thinking
  📌 Image support
  📌 Prompt caching
  ✅ Quiz on features of Claude

**Agents**
  📌 Agents overview

**Wrap up**
  📌 Course wrap up
  📌 Implementing the RAG flow
  📌 BM25 lexical search
  📌 A multi-search RAG pipeline
  📌 Reranking results
  📌 Contextual retrieval
  ✅ Quiz on Retrieval Augmented Generation
  📌 PDF support
  📌 Citations
  📌 Rules of prompt caching
  📌 Prompt caching in action
  📌 Introducing MCP
  📌 MCP clients
  📌 Project setup
  📌 Defining tools with MCP
  📌 The server inspector
  📌 Implementing a client
  📌 Defining resources
  📌 Accessing resources
  📌 Defining prompts
  📌 Prompts in the client
  📌 MCP review
  📌 Claude Code setup
  📌 Claude Code in action
  📌 Enhancements with MCP servers
  📌 Parallelizing Claude Code

**Model Context Protocol**
  ✅ Quiz on Model Context Protocol

**Final assessment**
  ✅ Final assessment quiz

---

## 📖 Lesson Content


### 📖 Course introduction

#### 1. Introduction to the course

Welcome to the course! This module will help you:
- Understand the course structure and learning path
- Identify the key skills and knowledge areas covered in the course
- Recognize the intended audience and assess personal readiness for the course

#### 2. Overview of Claude Models

Claude offers three distinct model families, each optimized for different priorities. All three models share Claude's core capabilities - they can handle text generation, coding, image analysis, and other tasks. The key difference is how they balance intelligence, speed, and cost.

Claude Opus
Opus delivers Claude's highest level of intelligence. It's designed for complex scenarios that require sophisticated reasoning and planning capabilities.
Opus excels at working independently on complex projects for extended periods. It can manage multi-step processes and navigate different requirements without much human intervention. The model supports reasoning, meaning it can provide quick responses for simple tasks or spend time thinking through more complex problems.
The trade-off is moderate latency and higher cost. You're paying more and waiting longer for that extra intelligence.

Claude Sonnet
Sonnet sits in the sweet spot of Claude's lineup, offering a balanced combination of intelligence, speed, and cost that works well for most practical applications.
What makes Sonnet particularly valuable is its strong coding ability combined with fast text generation. Many developers appreciate its ability to make precise edits to complex codebases without breaking existing functionality.

Claude Haiku
Haiku is Claude's fastest model, built specifically for applications where response time is critical. It's optimized for speed and cost efficiency rather than maximum intelligence.
One important limitation: Haiku doesn't support the reasoning capabilities that Opus and Sonnet offer. This makes it ideal for user-facing applications that need real-time interactions but less suitable for complex problem-solving tasks.

Choosing the Right Model
Model selection comes down to understanding the trade-offs between intelligence and cost/speed. Here's how to decide:
- Choose Opus: when intelligence is your top priority. If you have complex tasks requiring strong reasoning capabilities, you're choosing quality over speed and cost.
- Choose Haiku: when speed matters most. For real-time user interactions or high-volume processing where you need the fastest possible responses.
- Choose Sonnet: when you need balance. Most applications benefit from Sonnet's combination of intelligence, speed, and reasonable cost.

Using Multiple Models
Many teams don't stick to just one model. Instead, they use different models for different parts of the same application:

> *(See full lesson at course URL)*


### 📖 Working with the API

#### 3. Accessing the API

When building applications with AI models, you need to understand the flow of data from user input to AI-generated response. Let's walk through how this works with AWS Bedrock and see what happens behind the scenes of a typical chat application.

How Chat Applications Work
Imagine you're building a web app with a simple chat interface. A user types 'Define quantum computing' and clicks send. Here's what actually happens:

The user sees a clean interface, but there's a whole system working behind the scenes to generate that response.

The Request Flow
When a user submits text, here's the journey that message takes:
- User submits their message through your web interface
- Your server receives the request containing that text
- Your server uses the Bedrock client to make a request to AWS Bedrock
- The request includes the user message and a model ID (like Claude Haiku or Claude Sonnet)
- The chosen model processes the request and generates text
- AWS Bedrock sends back an assistant message containing the generated response
- Your server forwards this response back to the user's browser

#### 4. Making a request

Making your first API request to AWS Bedrock requires three essential components: a Bedrock Runtime Client to connect to the service, a Model ID to specify which model you want to run, and a User Message containing the text you want to feed into the model.

Setting Up the Bedrock Client
Start by creating a client using boto3 to connect to the Bedrock runtime service:
import boto3
client = boto3.client('bedrock-runtime', region_name='us-west-2')

Understanding Model IDs and Regional Availability
Here's where things get tricky. Not every model is available in every AWS region. If you try to run a model that doesn't exist in your chosen region, you'll get a cryptic error message saying the model doesn't exist.

For example, if Claude Sonnet is available in us-west-2 but you're making requests from us-east-1, your request will fail.

Using Inference Profiles
Inference profiles solve the regional availability problem by automatically routing your requests to a region where your chosen model is actually hosted.

Instead of tracking which models are in which regions, you can use an inference profile that knows the model is available in multiple regions like us-west-2 and us-east-2.

When you make a request using an inference profile, AWS automatically routes it to the correct region where your model exists, even if you're connecting from a different region.
To find inference profile IDs, go to the AWS Bedrock console and look under 'Cross-region inference' rather than using the model ID from the main model catalog page.

Copy the inference profile ID for your chosen model.

Creating User Messages
User messages have a specific structure that might look overly complex at first, but there's a good reason for it:
user_message = {
  'role': 'user',
  'content': [ {'text': "What's 1+1?"} ]
}

The content is a list because a single message can contain different types of content - text, images, or other media types. This structure allows you to send multimodal requests.

Making the Request
Now you can make your API call using the converse method:
response = client.converse(
    modelId=model_id,
    messages=[user_message]
)

The response contains a lot of metadata, but to get just the generated text, you need to navigate through the response structure:
response['output']['message']['content'][0]['text']

Understanding Message Types
There are two main message types you'll work with:
- User messages - Content you want to feed into the model (role: 'user')

> *(See full lesson at course URL)*

#### 5. Multi-Turn conversations

The code we've written so far simulates a very simple exchange with Claude. But what happens when you want to continue a conversation? When you ask a follow-up question like 'And 3 more?' after asking 'What's 1+1?', you might expect Claude to understand you're asking about adding 3 to the previous result of 2.

However, there's something critical you need to understand about the Bedrock API and Claude itself.

No Message Storage
Bedrock and Claude do not store any messages. None of the messages you send get stored, and none of the responses you receive are stored either. Each API call is completely independent.

To have a conversation with multiple messages that maintain context, you need to:
- Manually maintain a list of all messages in your code
- Provide that entire list of messages with each follow-up request

Why Context Matters
Let's see what happens without proper context. If you send just 'And 3 more?' as a standalone message, Claude has no idea what you're referring to. It will do its best to respond, but the answer won't make sense because it lacks the context of your previous conversation.

When you send only the follow-up question, Claude sees just that isolated message and tries to respond without knowing about the previous 'What's 1+1?' exchange.

Building Conversation Context
To maintain context, you need to include the full conversation history in each request. Here's how it works:

Your message list should contain all previous exchanges - both user messages and assistant responses. When you send this complete context, Claude can understand that 'And 3 more?' refers to adding 3 to the previous result of 2.

Helper Functions for Message Management
To make conversation management easier, you can create helper functions:
def add_user_message(messages, text):
    user_message = {
        'role': 'user',
        'content': [ {'text': text} ]
    }
    messages.append(user_message)

def add_assistant_message(messages, text):
    assistant_message = {
        'role': 'assistant',
        'content': [ {'text': text} ]
    }
    messages.append(assistant_message)

def chat(messages):
    response = client.converse(
        modelId=model_id,
        messages=messages
    )
    return response['output']['message']['content'][0]['text']

Implementing Multi-Turn Conversations
Here's how to build a conversation step by step:
# Make a starting list of messages
messages = []

# Add in the initial user question of 'What's 1+1?'

> *(See full lesson at course URL)*

#### 6. Chat bot exercise

VIDEO-ONLY: Exercise lesson with no additional text content below the video.

#### 7. System prompts

When building AI chatbots for specific use cases, you need a way to control how the AI responds. System prompts are the key to transforming a general-purpose AI into a specialized assistant that follows specific guidelines and stays on topic.

The Problem with User-Level Instructions

You might think the solution is to include all your requirements in the user message itself. For example, telling the AI in each conversation to "mention AWS services" and "don't mention competitors." This approach has serious limitations:
• You'd need to anticipate every possible question and edge case
• The instruction list becomes unwieldy and repetitive
• Users see all the internal instructions, making conversations cluttered
• Requirements change based on the specific question being asked

System Prompts: A Better Approach

System prompts solve this problem by giving Claude a role to play. Instead of listing specific do's and don'ts, you tell Claude to act like a particular type of professional. The AI then responds as that person would naturally respond.

System prompts provide several key benefits:
• Claude gets guidance on how to respond consistently
• The AI adopts the mindset and constraints of the specified role
• Responses stay focused and on-brand automatically
• You don't need to anticipate every possible scenario

Implementing System Prompts

To add a system prompt to your Claude conversation, you pass it as a parameter to the converse function:

system_prompt = """You are an AWS cloud support specialist. Your job is to answer user queries related to cloud hosting services on AWS."""

response = client.converse(
    modelId=model_id,
    messages=messages,
    system=[{"text": system_prompt}]
)

The system prompt gets passed as a list containing a dictionary with a "text" key. This tells Claude what role to adopt before it sees any user messages.

Building a Flexible Chat Function

Here's a reusable chat function that handles system prompts elegantly:

def chat(messages, system=None):
    params = {"modelId": model_id, "messages": messages}
    if system:
        params["system"] = [{"text": system}]
    response = client.converse(**params)
    return response["output"]["message"]["content"][0]["text"]

This approach lets you optionally include a system prompt. When no system prompt is provided, Claude responds as its default self. When you include one, Claude adopts that specific role.

System Prompts in Action


> *(See full lesson at course URL)*

#### 8. System prompt exercise


#### 9. Temperature

Temperature is a powerful parameter that controls how creative or deterministic Claude's responses will be. Understanding how to use it effectively can dramatically improve your AI applications.

How Claude Generates Text

Before diving into temperature, it's helpful to understand Claude's text generation process. When you send Claude a prompt like "What do you think?", it goes through three phases:

• Tokenization: Breaking your input into smaller chunks
• Prediction: Calculating probabilities for possible next tokens
• Sampling: Selecting a token based on those probabilities

In the diagram above, you can see how Claude might assign different probabilities to potential next tokens. The word "about" has a 30% chance, "would" has 20%, and so on. This process repeats for each token until the response is complete.

What Temperature Does

Temperature is a decimal value between 0 and 1 that directly influences these token selection probabilities. Think of it as a creativity dial:

• Low temperature (near 0): Makes the highest probability token much more likely to be selected
• High temperature (near 1): Distributes probability more evenly across all possible tokens

At temperature 0, Claude becomes deterministic - it will always pick the most probable token. At temperature 1, lower-probability tokens have a much better chance of being selected, leading to more creative and varied outputs.

Temperature Ranges and Use Cases

Different tasks call for different temperature settings:

Low Temperature (0.0 - 0.3):
• Factual responses
• Coding assistance
• Data extraction
• Content moderation

Medium Temperature (0.4 - 0.7):
• Summarization
• Educational content
• Problem-solving
• Creative writing with constraints

High Temperature (0.8 - 1.0):
• Brainstorming
• Creative writing
• Marketing content
• Joke generation

Setting Temperature in Code

By default, Claude's temperature is set to 1.0, which means maximum creativity. You can override this by adding temperature to your inference configuration:

def chat(messages, system=None, temperature=1.0):
    params = {
        "modelId": model_id,
        "messages": messages,
        "inferenceConfig": {"temperature": temperature}
    }
    if system:
        params["system"] = [{"text": system}]
    response = client.converse(**params)
    return response["output"]["message"]["content"][0]["text"]

Temperature in Practice


> *(See full lesson at course URL)*

#### 10. Streaming

When building chat interfaces with AI models, users expect to see responses appear immediately rather than waiting 10-30 seconds for a complete response. The converse_stream function solves this by streaming text as it's generated, creating a much better user experience.

How Streaming Works

Instead of waiting for the entire response to be generated, streaming sends back pieces of text as soon as they're available. Here's how the flow changes:

When you call converse_stream, you immediately get back an initial response that contains a stream object. This stream is a generator that yields events as the model generates text. Each event contains a small chunk of the overall response.

Basic Implementation

Here's how to use converse_stream in your code:

messages = []
add_user_message(messages, "Write a 1 sentence description of a fake database")
response = client.converse_stream(messages=messages, modelId=model_id)
for event in response["stream"]:
    print(event)

This will print out all the different events as they arrive. You'll see the response come in chunks rather than all at once.

Understanding Stream Events

The stream yields several types of events, each serving a different purpose:

For basic text generation, you only need to care about contentBlockDelta events. These contain the actual generated text chunks that you want to display to users.

The events always arrive in a predictable order: messageStart, multiple contentBlockDelta events containing your text, then contentBlockStop, messageStop, and finally metadata.

Extracting the Text

To get just the generated text from each chunk, filter for contentBlockDelta events and extract the text:

text = ""
for event in response["stream"]:
    if "contentBlockDelta" in event:
        chunk = event["contentBlockDelta"]["delta"]["text"]
        print(chunk, end="")
        text += chunk
print("\n\nTotal Message:\n" + text)

The end="" parameter removes the automatic newline that Python's print function adds, making the streaming text appear more naturally.

Practical Applications

In a real application, instead of printing each chunk, you'd typically:

• Send each chunk to your frontend via WebSockets or Server-Sent Events
• Update the UI to display the growing response in real-time
• Store the complete message once streaming finishes
• Handle any errors that might occur during streaming


> *(See full lesson at course URL)*

#### 11. Controlling model output

Beyond crafting better prompts, there are two powerful techniques for controlling Claude's output: prefilled assistant messages and stop sequences. These methods give you precise control over how Claude responds and when it stops generating text.

Prefilled Assistant Messages

Message prefilling lets you provide the beginning of Claude's response, which strongly influences the direction of its answer. Instead of letting Claude decide how to start its response, you give it a specific opening that steers the conversation.

Here's how it works: you build your normal list of messages with the user's question, but then add an assistant message at the end containing the start of the response you want. When Claude processes this, it sees the assistant message and thinks "I've already started responding to this question, so I should continue from where I left off."

For example, if you ask "Is tea or coffee better at breakfast?" and prefill with "Coffee is better because", Claude will continue from that point and build a response supporting coffee. The key insight is that Claude will pick up exactly where your prefilled text ends - it won't repeat what you've written.

Let's see this in practice:

messages = []
add_user_message(messages, "Is coffee or tea better for breakfast?")
add_assistant_message(messages, "Coffee is better because")
chat(messages)

This returns something like "it has more caffeine." Notice that Claude continues directly from your prefilled text, so you'll need to combine both parts to get the complete response: "Coffee is better because it has more caffeine."

You can steer Claude in any direction by changing your prefilled text:
• "Tea is better because" - pushes toward tea
• "They are the same because" - creates a neutral response

Stop Sequences

Stop sequences force Claude to end its response immediately when it generates specific text. This is useful when you want to truncate output at a particular point or prevent Claude from continuing past a certain marker.

The concept is straightforward: you provide a list of strings, and as soon as Claude generates any of those strings, it stops and returns whatever it has generated so far. The stop sequence itself is not included in the response.

To use stop sequences, you need to modify your chat function to accept them as a parameter:

def chat(messages, system=None, temperature=1.0, stop_sequences=[]):
    params = {
        "modelId": model_id,
        "messages": messages,

> *(See full lesson at course URL)*

#### 12. Structured data

When you need Claude to generate structured data like JSON, Python code, or bulleted lists, you'll often run into a common problem: Claude wants to be helpful and add explanatory text, headers, or markdown formatting around your content. This extra commentary breaks the user experience when you just need the raw data.

Consider building a web app that generates AWS EventBridge rules. Users enter a description, click generate, and expect to see clean JSON they can immediately copy and use. If Claude returns the JSON wrapped in markdown code blocks with explanatory text, users can't simply copy the entire response - they have to manually select just the JSON portion.

The Problem with Default Responses

By default, Claude tends to format structured output like this:

# EventBridge Rule

```json
{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": {"state": ["running"]}
}
```

This rule captures EC2 instance state changes when instances start running or stop.

While this is great for documentation, it's problematic when you need just the JSON for programmatic use.

The Solution: Assistant Message Prefilling + Stop Sequences

You can combine assistant message prefilling with stop sequences to get exactly the content you want. Here's how it works:

messages = []
add_user_message(messages, "Generate a very short event bridge rule as json")
add_assistant_message(messages, "```json")
text = chat(messages, stop_sequences=["```"])

This technique works by:
• Prefilling the assistant message with the opening markdown delimiter
• Setting a stop sequence to halt generation when Claude tries to close the code block
• Capturing only the content between these delimiters

How It Works Behind the Scenes

When Claude receives your request, it sees the prefilled assistant message and assumes it already started writing the JSON code block. Instead of adding its own header and opening delimiter, Claude jumps straight to generating the actual JSON content.

When Claude finishes the JSON and naturally wants to close the markdown code block with ```, the stop sequence immediately halts generation and returns the response. You get just the JSON content with no extra formatting.

Processing the Results

The returned text might contain some newline characters, but you can easily clean this up:

import json
# Parse as JSON to validate and format
parsed_data = json.loads(text.strip())
# Or just strip whitespace for other data types

> *(See full lesson at course URL)*

#### 13. Structured data exercise


#### 14. Quiz on working with the API



### 📖 Prompt evaluations

#### 15. Prompt evaluation

When working with Claude, writing a good prompt is just the beginning. To build reliable AI applications, you need to understand two critical concepts: prompt engineering and prompt evaluation. Prompt engineering gives you techniques for crafting better prompts, while prompt evaluation helps you measure how well those prompts actually work.

Prompt Engineering vs Prompt Evaluation

Prompt engineering is your toolkit for writing and improving prompts. It's a set of best practices that help Claude understand exactly what you're asking for and how you want it to respond. Think of it as the craft of prompt writing - techniques like multishot prompting, structuring with XML tags, and many other approaches we'll explore.

Prompt evaluation, on the other hand, is about measurement. It's automated testing that gives you objective metrics on whether your prompts are actually effective. Instead of guessing if your prompt works well, evaluation lets you:
• Test against expected answers
• Compare different versions of the same prompt
• Review outputs for errors

The Three Paths After Writing a Prompt

Once you've drafted a prompt, you typically face three options for what to do next:

Option 1: Test the prompt once and decide it's good enough. This carries a significant risk of breaking in production when users provide unexpected inputs.

Option 2: Test the prompt a few times and tweak it to handle a corner case or two. While better than option 1, this approach still leaves you vulnerable because users will often provide very unexpected outputs that you haven't considered.

Option 3: Run the prompt through an evaluation pipeline to score it, then iterate on the prompt based on objective data. This requires more work and cost upfront, but gives you much more confidence in your prompt's reliability.

Why Most Engineers Fall Into Testing Traps

Options 1 and 2 are traps that all engineers fall into, myself included. It's natural to write a prompt for a serious application and not test it thoroughly enough. We tend to test with inputs that seem obvious to us, but real users will interact with your prompts in ways you never anticipated.

The solution is to embrace option 3: systematic evaluation. By running your prompts through proper evaluation pipelines, you get objective scores that tell you how well your prompt performs across a wide range of scenarios. This data-driven approach lets you iterate confidently and catch issues before they reach production.


> *(See full lesson at course URL)*

#### 16. A typical eval workflow

A typical prompt evaluation workflow follows a systematic approach to objectively measure and improve your prompts. While there are many different ways to assemble these workflows and various open source and paid tools available, understanding the core process helps you start small and scale up as needed.

Step 1: Draft Your Initial Prompt

Start by writing out a basic prompt that you want to improve. For this example, we'll use a simple prompt structure:

prompt = f""" Please answer the user's question: {question} """

This gives us a baseline to work from. We won't know if it's effective until we evaluate it with some objective methodology.

Step 2: Create an Evaluation Dataset

Your evaluation dataset contains sample inputs that you'll feed into your prompt. Since our prompt only has one input (the user's question), we need a collection of different questions to test with.

The dataset contains questions that we will merge with our prompt. You can assemble these datasets by hand or generate them using Claude. In real-world evaluations, you might have tens, hundreds, or even thousands of different records, but we'll start with just three questions for this example:
• What's 2+2?
• How do I make oatmeal?
• How far away is the Moon?

Step 3: Feed Through Claude

Take each question from your dataset and merge it with your prompt template to create complete prompts. Then send each one to Claude and collect the responses.

For example, the first question becomes a complete prompt that Claude can respond to. You'll repeat this process for all records in your dataset, getting back responses like "2 + 2 = 4", detailed oatmeal instructions, and information about the Moon's distance.

Step 4: Feed Through a Grader

Now comes the crucial step: objectively scoring Claude's responses. Take each question-answer pair and feed them into a grader that will evaluate the quality of Claude's response.

The grader assigns scores (typically 1-10) based on response quality:
• 10 = Perfect answer, no room for improvement
• 4 = Definitely room for improvement
• 1 = Poor or incorrect response

In our example, the responses might score 10, 4, and 9 respectively. Average these scores together to get an overall performance metric: 7.66.

Step 5: Change Prompt and Repeat

With your baseline score established, you can now iterate on your prompt. Try adding more specific instructions to guide Claude's responses:


> *(See full lesson at course URL)*

#### 17. Generating test datasets

Building a custom prompt evaluation workflow starts with creating a clear goal and generating test data. In this case, we're building a prompt that helps users write AWS-specific code - either Python functions, JSON configurations, or regular expressions - with no extra explanations or formatting.

Setting Up the Goal

The prompt should take a user's task description and return one of three output types:
• Python code
• JSON configuration
• Regular expressions

The key requirement is that responses should contain only the requested code without headers, footers, or explanations.

Starting with a simple first version keeps things manageable. The initial prompt template is straightforward: "Please provide a solution to the following task: {task}"

Creating Evaluation Datasets

An evaluation dataset contains input examples that you'll feed into your prompt. Each test case gets combined with your prompt and sent to Claude, letting you see how well the prompt performs across different scenarios.

You can create datasets in two ways:
• Manually write test cases by hand
• Generate them automatically using Claude

For automatic generation, using a faster model like Haiku makes sense since you're generating multiple test cases.

Generating Test Data with Code

The dataset generation function uses Claude to create realistic test scenarios. Here's the basic structure:

def generate_dataset():
    prompt = """ Generate 3 AWS-related tasks that require Python, JSON, or Regex solutions. Focus on tasks that can be solved by writing a single Python function, a single JSON object, or tasks that do not require writing much code. Example output: [ { "task": "Description of task" }, ...additional ] Please generate 3 objects. """
    messages = []
    add_user_message(messages, prompt)
    add_assistant_message(messages, "```json")
    text = chat(messages, stop_sequences=["```"])
    return json.loads(text)

This approach uses the pre-filled assistant message technique with stop sequences to extract clean JSON responses. The assistant message starts with "```json" and stops at the closing "```", ensuring you get properly formatted data.

Saving Your Dataset

Once generated, save the dataset to avoid regenerating it constantly:

dataset = generate_dataset()
with open("dataset.json", "w") as f:
    json.dump(dataset, f, indent=2)


> *(See full lesson at course URL)*

#### 18. Running the eval

Now that we have our evaluation dataset ready, it's time to build the core evaluation pipeline. This involves taking each test case, merging it with our prompt, feeding it to Claude, and then grading the results.

The evaluation process follows a clear workflow: we take our dataset of test cases, combine each one with our prompt template, send it to Claude for processing, and then evaluate the output using a grader system.

Building the Core Functions

The evaluation pipeline consists of three main functions, each with a specific responsibility. Let's start with the simplest one - the function that handles individual prompt execution.

The run_prompt Function

This function takes a test case and merges it with our prompt template:

def run_prompt(test_case):
    """Merges the prompt and test case input, then returns the result"""
    prompt = f""" Please solve the following task: {test_case["task"]} """
    messages = []
    add_user_message(messages, prompt)
    output = chat(messages)
    return output

Right now, we're keeping the prompt extremely simple. We're not including any formatting instructions, which means Claude will likely return more verbose output than we need. We'll refine this later as we iterate on our evaluation process.

The run_test_case Function

This function orchestrates running a single test case and grading the result:

def run_test_case(test_case):
    """Calls run_prompt, then grades the result"""
    output = run_prompt(test_case)
    # TODO - Grading
    score = 10
    return {
        "output": output,
        "test_case": test_case,
        "score": score
    }

For now, we're using a hardcoded score of 10. The grading logic is where we'll spend significant time in upcoming sections, but this placeholder lets us test the overall pipeline structure.

The run_eval Function

This is the main orchestrator that processes the entire dataset:

def run_eval(dataset):
    """Loads the dataset and calls run_test_case with each case"""
    results = []
    for test_case in dataset:
        result = run_test_case(test_case)
        results.append(result)
    return results

This function loops through every test case in our dataset, processes each one, and collects all the results into a single list.

Running the Evaluation

To execute our evaluation pipeline, we load the dataset and call our main function:

with open("dataset.json", "r") as f:
    dataset = json.load(f)
results = run_eval(dataset)


> *(See full lesson at course URL)*

#### 19. Model based grading

When building prompt evaluation workflows, graders provide objective signals about output quality. A grader takes model output and returns some kind of measurable feedback - typically a number between 1-10, where 10 represents high quality and 1 represents poor quality.

Types of Graders

There are three main approaches to grading model outputs:

Code graders - Programmatically evaluate outputs using custom logic

Model graders - Use another AI model to assess quality

Human graders - Have people manually review and score outputs

Code Graders

Code graders let you implement any programmatic check you can imagine. Common uses include:
• Checking output length
• Verifying output does/doesn't have certain words
• Syntax validation for JSON, Python, or regex
• Readability scores

The only requirement is that your code returns some measurable signal when it runs.

Model Graders

Model graders make an additional API request to evaluate the original output. This approach offers tremendous flexibility for assessing:
• Response quality
• Quality of instruction following
• Completeness
• Helpfulness
• Safety

Human Graders

Human graders provide the most flexibility but are time-intensive and tedious. They're useful for evaluating:
• General response quality
• Comprehensiveness
• Depth
• Conciseness
• Relevance

Defining Evaluation Criteria

Before implementing any grader, you need clear evaluation criteria. For a code generation prompt, you might focus on:

Format - Should return only Python, JSON, or Regex without explanation

Valid Syntax - Produced code should have valid syntax

Task Following - Response should directly address the user's task with accurate code

The first two criteria work well with code graders, while task following is better suited for model graders due to their flexibility.

Implementing a Model Grader

Here's how to build a model grader function:

def grade_by_model(test_case, output):
    # Create evaluation prompt
    eval_prompt = """ You are an expert code reviewer. Evaluate this AI-generated solution.
    Task: {task}
    Solution: {solution}
    Provide your evaluation as a structured JSON object with:
    - "strengths": An array of 1-3 key strengths
    - "weaknesses": An array of 1-3 key areas for improvement
    - "reasoning": A concise explanation of your assessment
    - "score": A number between 1-10 """
    messages = []
    add_user_message(messages, eval_prompt)
    add_assistant_message(messages, "```json")

> *(See full lesson at course URL)*

#### 20. Code based grading

Code-based grading adds an extra layer of validation to your prompt evaluations by checking whether the model's output follows the correct format and has valid syntax. This is especially useful when you're asking models to generate code, JSON, or regular expressions.

How Code Grading Works

The code grader evaluates two main criteria:

Format compliance - Does the output contain only the requested format (Python, JSON, or regex) without explanations?

Valid syntax - Can the output actually be parsed or compiled successfully?

The system uses separate validation functions for each format type. If the code parses successfully, it gets a perfect score of 10. If parsing fails with an error, it gets a score of 0.

Setting Up Validation Functions

You'll need three helper functions to validate different output types:

def validate_json(text):
    try:
        json.loads(text.strip())
        return 10
    except json.JSONDecodeError:
        return 0

def validate_python(text):
    try:
        ast.parse(text.strip())
        return 10
    except SyntaxError:
        return 0

def validate_regex(text):
    try:
        re.compile(text.strip())
        return 10
    except re.error:
        return 0

These functions use Python's built-in parsing capabilities to check syntax validity. The json.loads() function validates JSON, ast.parse() creates a Python abstract syntax tree, and re.compile() validates regular expressions.

Adding Format Information to Test Cases

Your test dataset needs to specify the expected output format for each task. Update your dataset generation prompt to include a format field:

{ "task": "Description of task", "format": "python" }

The format field should contain "json", "python", or "regex" depending on what type of output you expect from that particular task.

Improving Your Prompt

To get better results from the code grader, make your prompt instructions more specific:

* Respond only with Python, JSON, or a plain Regex
* Do not add any comments or commentary or explanation

You can also use a pre-filled assistant message with code blocks and stop sequences to ensure clean output formatting.

Combining Scores

The final step is merging your model grader score with the syntax grader score. A simple approach is to take the average of both scores:

model_grade = grade_by_model(test_case, output)
model_score = model_grade["score"]
syntax_score = grade_syntax(output, test_case)
score = (model_score + syntax_score) / 2


> *(See full lesson at course URL)*

#### 72. Exercise on prompt evals

## Exercise on prompt evals
In this exercise, you'll create and run a prompt evaluation to compare two different system prompts. This hands-on practice will help you understand how to systematically assess the quality of your prompts.

## The Challenge
You will be working with two different system prompts for the same task - generating product descriptions from a product name and category. The goal is to determine which prompt produces better results.

## Background
Prompt evaluation is essential for building reliable AI applications. By systematically comparing prompts, you can identify which approaches work best for your specific use case.

## What You'll Do
- : Create two different system prompts for the product description task
- : Generate a test dataset using Claude
- : Run evaluations on both prompts
- : Analyze the results to determine which prompt performs better
This exercise uses model-based grading to evaluate the outputs, similar to what you learned in the earlier lessons on prompt evaluation.

## Learning Objectives
- : Apply the prompt evaluation workflow in a practical scenario
- : Compare outputs from different prompt strategies
- : Make data-driven decisions about prompt design

#### 73. Quiz on prompt evaluations

## Quiz on prompt evaluations

## Quiz on Prompt Evaluations


### 📖 Prompt engineering

#### 21. Prompt engineering

Prompt engineering is about taking a prompt you've written and improving it to get more reliable, higher-quality outputs. This process involves writing an initial prompt, evaluating its performance, then systematically applying engineering techniques to improve it step by step.

The Iterative Improvement Process

The approach follows a clear cycle: set a goal, write an initial prompt, evaluate it, apply a prompt engineering technique, then re-evaluate to verify better performance. This cycle repeats as you refine your prompt.

For this tutorial series, we'll work on a practical example: creating a prompt that generates one-day meal plans for athletes based on their height, weight, physical goals, and dietary restrictions.

Setting Up the Evaluation Pipeline

The evaluation uses an improved version of the pipeline from previous modules, wrapped in a PromptEvaluator class that handles dataset generation and model grading. The class supports concurrency to speed up the evaluation process:

evaluator = PromptEvaluator(max_concurrent_tasks=5)

Start with a low concurrency value (like 3) to avoid rate limit errors. You can adjust this based on your API quota.

Generating Test Data

The generate_dataset method creates test cases for your prompt. You need to specify:
• A task description explaining what your prompt should do
• A specification of the inputs your prompt requires
• The number of test cases to generate

For the meal planning example:

dataset = evaluator.generate_dataset(
    task_description="Write a compact, concise 1 day meal plan for a single athlete",
    prompt_inputs_spec={
        "height": "Athlete's height in cm",
        "weight": "Athlete's weight in kg",
        "goal": "Goal of the athlete",
        "restrictions": "Dietary restrictions of the athlete"
    },
    num_cases=3
)

Writing Your Initial Prompt

Start with a simple, naive prompt to establish a baseline. The run_prompt function receives the test case inputs and should return the model's response:

def run_prompt(prompt_inputs):
    prompt = f""" What should this person eat?
    - Height: {prompt_inputs["height"]}
    - Weight: {prompt_inputs["weight"]}
    - Goal: {prompt_inputs["goal"]}
    - Dietary restrictions: {prompt_inputs["restrictions"]} """
    messages = []
    add_user_message(messages, prompt)
    return chat(messages)

Running the Evaluation


> *(See full lesson at course URL)*

#### 22. Being clear and direct

The first line of your prompt is the most important part of your entire request. This is where you set the stage for everything that follows, and getting it right can dramatically improve your results.

Being Clear and Direct

When crafting that crucial first line, you want to focus on two key principles: clarity and directness. This means using simple language that leaves no room for ambiguity about what you want Claude to do.

Clear Communication

Being "clear" means:
• Use simple language that anyone can understand
• State exactly what you want without beating around the bush
• Lead with a straightforward statement of Claude's task

Instead of writing something vague like "I need to know about those things people put on their roofs that use sun - those solar panel things, I think they're called," be direct and write: "Write three paragraphs about how solar panels work."

Direct Instructions

Being "direct" focuses on how you structure your request:
• Use instructions, not questions
• Start with direct action verbs like "Write," "Create," or "Generate"

Rather than asking "I was reading about renewable energy and geothermal energy sounds neat. What countries use it?" try: "Identify three countries that use geothermal energy. Include generation stats for each."

Putting It Into Practice

Let's see this technique in action. Starting with a weak prompt that simply asked "What should this person eat?" we can apply our clear and direct approach. The improved version becomes:

Generate a one-day meal plan for an athlete that meets their dietary restrictions.

This revision immediately tells Claude:
• What action to take (generate)
• What to create (a meal plan)
• Key constraints (one day, for an athlete, considering restrictions)

Results Matter

This simple change can make a significant difference in output quality. In our example, the evaluation score jumped from 2.32 to 3.92 - a substantial improvement from just restructuring that opening line.

The key takeaway is that Claude responds best when you treat it like a capable assistant who needs clear direction rather than someone who has to guess what you want. Start strong with a direct action verb, be specific about the task, and you'll see better results right away.

#### 23. Being specific

When working with Claude, one of the most effective ways to improve your results is to be specific about what you want. Instead of leaving everything up to the model's interpretation, you can provide clear guidelines or steps that direct Claude toward the kind of output you're looking for.

Think about it this way: if you ask Claude to "write a short story about a character who discovers a hidden talent," the model could go in countless directions. It might write 200 words or 2,000 words. It could focus on one character or introduce five. The story structure could vary wildly.

But if you add specific guidelines, you can shape the output to match your needs much more closely.

Two Types of Guidelines

There are two main approaches to being specific in your prompts, and you'll often see both used together in professional applications.

Quality Guidelines

The first type focuses on listing qualities that your output should have. These guidelines control attributes like:
• Length constraints (keep under 1,000 words)
• Structural requirements (include a clear action that reveals the character's talent)
• Content specifications (include at least one supporting character)

Process Steps

The second type provides specific steps for the model to follow. This approach makes Claude think through the problem systematically:
• Brainstorm 3 talents that would create dramatic tension
• Pick the most interesting talent
• Outline a pivotal scene that reveals the talent
• Brainstorm 3 supporting character types that could increase the impact of this discovery

Quality guidelines control what the output looks like, while process steps control how Claude arrives at that output.

Real-World Testing

Let's look at how this works in practice. Here's a prompt for generating meal plans that incorporates specific guidelines:

Generate a one-day meal plan for an athlete that meets their dietary restrictions.
- Height: {prompt_inputs["height"]}
- Weight: {prompt_inputs["weight"]}
- Goal: {prompt_inputs["goal"]}
- Dietary restrictions: {prompt_inputs["restrictions"]}

Guidelines:
1. Include accurate daily calorie amount
2. Show protein, fat, and carb amounts
3. Specify when to eat each meal
4. Use only foods that fit restrictions
5. List all portion sizes in grams
6. Keep budget-friendly if mentioned

When tested against a baseline prompt without guidelines, this approach improved the evaluation score from 3.92 to 7.86 - more than doubling the quality.


> *(See full lesson at course URL)*

#### 24. Structure with XML tags

When you're building prompts that include a lot of content, Claude can sometimes struggle to understand which pieces of text belong together or what different sections are supposed to represent. XML tags provide a simple way to add structure and clarity to your prompts, especially when you're interpolating large amounts of data.

Why Structure Matters

Consider a prompt where you need to analyze 20 pages of sales records. Without clear boundaries, Claude might have trouble distinguishing between your instructions and the actual data you want analyzed.

The example above shows how unclear boundaries can make it difficult for Claude to parse your intent. By wrapping the sales records in XML tags, you create clear separation between different parts of your prompt.

Using XML Tags for Clarity

XML tags act as delimiters that help Claude understand the structure of your prompt. You can create custom tag names that describe the content they contain:

<sales_records> {sales_records} </sales_records>

The tag names don't need to follow any official XML specification - you're free to create descriptive names like sales_records, data, or records. More specific names generally work better than generic ones.

A Practical Example

Here's a clear example of why XML tags make a difference. In the "Not Great" version, it's unclear what content represents the buggy code versus the documentation. The improved version uses XML tags to clearly separate the different types of content:

<my_code>
from datavortex import Pipeline, DataSource

def process_data(input_file, output_file):
    pipeline = Pipeline()
    source = DataSource.from_csv(input_file)
</my_code>

<docs>
# Creating a data source from data vortex
csv_source = DataSource.from_csv("data.csv")
</docs>

Now Claude can easily distinguish between the code that needs debugging and the documentation that should guide the debugging process.

Applying Structure to Your Prompts

Even when your interpolated content isn't massive, XML tags can still improve clarity. For example, when generating meal plans, you can group athlete information together:

<athlete_information>
- Height: {prompt_inputs["height"]}
- Weight: {prompt_inputs["weight"]}
- Goal: {prompt_inputs["goal"]}
- Dietary restrictions: {prompt_inputs["restrictions"]}
</athlete_information>

This makes it crystal clear to Claude that this block contains external input about the athlete that should inform the meal plan generation.

When to Use XML Tags


> *(See full lesson at course URL)*

#### 25. Providing examples

Providing examples in your prompts is one of the most effective prompt engineering techniques you'll use. This approach, known as "one-shot" or "multi-shot" prompting, involves giving Claude sample input/output pairs to guide its responses.

How Examples Work

Let's look at a sentiment analysis example. Say you want Claude to categorize whether a tweet is positive or negative:

The challenge here is sarcasm. A tweet like "Yeah, sure, that was the best movie I've seen since 'Plan 9 from Outer Space'" appears positive on the surface, but it's actually sarcastic and negative (Plan 9 from Outer Space is famously terrible).

Adding Examples to Your Prompt

To handle this, you can add examples that show Claude exactly how to respond:

The key elements are:
• Clear introduction: "Here is a example input with an ideal response"
• XML tags for structure: <sample_input> and <ideal_output>
• Concrete examples that demonstrate the desired behavior

Handling Corner Cases

For tricky scenarios like sarcasm, you can provide multiple examples (multi-shot prompting). Add context to highlight what Claude should watch for:

Be especially careful with tweets that contain sarcasm. For example:

<sample_input> Oh yeah, I really needed a flight delay tonight! Excellent! </sample_input>
<ideal_output> Negative </ideal_output>

When to Use Examples

Examples are particularly useful for:
• Capturing corner cases or edge scenarios
• Defining complex output formats (like specific JSON structures)
• Showing Claude exactly what "good" output looks like

Finding Good Examples from Evaluations

When running prompt evaluations, look for your highest-scoring outputs in the HTML report. These make excellent examples to include in your prompt.

Find a response that scored well (ideally a 10, or your highest score), then copy both the input and output to use as your example.

Adding Context to Examples

You can make examples even more effective by explaining why they're good. After your example output, add a brief explanation:

<ideal_output> [Your example output here] </ideal_output>
This example meal plan is well-structured, provides detailed information on food choices and quantities, and aligns with the athlete's goals and restrictions.

This helps Claude understand not just what to produce, but why that output is considered ideal.

Best Practices

Use XML tags to clearly structure your examples
Be explicit about what you're showing Claude

> *(See full lesson at course URL)*

#### 74. Exercise on prompting

## Exercise on prompting
In this exercise, you'll practice the prompting techniques learned in the prompt engineering section. The exercise involves creating prompts that effectively guide Claude to generate specific types of responses.

## Exercise Overview
- : Practice clear and direct prompt writing
- : Apply XML tag structuring for better output control
- : Use examples to demonstrate desired response format
- : Iterate on prompts based on Claude's outputs

## What You'll Do
You'll work with a series of prompts designed to generate different types of content. Each prompt will test a specific technique from the course.

## Learning Objectives
- : Apply clear and direct prompt writing
- : Structure prompts with XML tags
- : Provide examples for better output quality
- : Evaluate and refine prompts iteratively

#### 75. Quiz on prompt engineering

## Quiz on prompt engineering

## Quiz on Prompt Engineering


### 📖 Tool use

#### 26. Introducing tool use

Tools allow Claude to access information from the outside world, solving one of its key limitations. By default, Claude only has access to information it was trained on, which means it can't provide current information like today's weather or recent news.

When a user asks "What's the weather in San Francisco, California?" Claude will typically respond with "I'm sorry, but I don't have access to up-to-date weather information." Tools fix this problem by creating a bridge between Claude and external data sources.

How Tool Use Works

The tool use process follows a specific flow that involves multiple back-and-forth communications between your server and Claude:
• Initial Request: You send Claude a question along with instructions on how to get extra data
• Tool Request: Claude analyzes the question and asks for specific external data it needs
• Data Retrieval: Your server runs code to fetch the requested information
• Final Response: Claude uses the external data to provide a complete, informed answer

Weather Example in Practice

Here's how the tool use flow works for a weather query:

When a user asks about weather, you include details on how to retrieve current weather data in your initial request to Claude. Claude recognizes it needs current weather information and asks your server to get it. Your server calls a weather API, retrieves the live data, and sends it back to Claude. Finally, Claude combines the original question with the fresh weather data to provide an accurate, current response.

Implementation Challenges

Tool use can feel confusing because there's a disconnect between the logical flow and how you actually write the code. The implementation doesn't follow the same order as the conceptual steps:

In practice, you often need to:
• Write the tool function first
• Create a JSON schema specification
• Handle the ToolUse and ToolResult parts
• Include the schema with your request

This jumping around between different parts of the implementation is why tool use initially seems complex. The key is understanding that each step in the logical flow requires specific code components that you'll build in a different order than they execute.

In the following videos, we'll implement tool use step by step, frequently referencing this flow diagram to keep track of which piece we're currently building.

#### 27. Tool functions

Building tools for Claude requires solving several challenges that aren't immediately obvious. When you want Claude to set reminders for future dates, you quickly discover that while Claude knows the current date, it doesn't always know the exact time, struggles with complex date arithmetic, and has no built-in way to actually set reminders.

The solution is to create custom tools that handle these specific tasks. For a reminder system, you'll need three separate tools: one to get the current date and time, another to add durations to dates, and a third to actually set the reminder.

Why This Is Challenging

Claude has some limitations when it comes to time-based tasks:
• Claude might know the current date, but not the exact time
• Claude doesn't always handle time-based addition well, especially when looking many days into the future
• Claude doesn't know how to set a reminder

The Tools You Need

To solve these problems, you'll create three dedicated tools:
• Get the current date time - Claude needs to know the current date and time
• Add duration to date time - Claude isn't perfect with date time addition
• Set a reminder - Need a way to set a reminder

How Tool Functions Work

The tool system follows a specific flow between your server and Claude. You write functions that Claude can call when it needs additional information, and Claude receives the results to help formulate its response.

The process involves several steps: writing the tool function, creating a JSON schema specification, calling Claude with that schema, running the tool when Claude requests it, and providing the results back to Claude.

Writing Tool Functions

Tool functions are plain Python functions that get executed when Claude decides it needs additional information to help the user. Here's how to write them effectively:

Best Practices:
• Use well-named, descriptive arguments (this becomes important later)
• Validate the inputs, raising an error if they fail validation
• Return meaningful errors - Claude will try to call your function a second time if it gets an error

Creating Your First Tool

Let's start with the simplest tool - getting the current date and time. This function takes a date format parameter and returns the current timestamp:

from datetime import datetime, timedelta

def get_current_datetime(date_format="%Y-%m-%d %H:%M:%S"):
    return datetime.now().strftime(date_format)


> *(See full lesson at course URL)*

#### 28. JSON Schema for tools

After creating your tool function, the next step is writing a JSON schema to describe it. This schema tells Claude what arguments your function expects and how to use it properly. While the configuration might look intimidating at first, it's actually straightforward once you understand the process.

Understanding JSON Schema

JSON Schema isn't something invented just for AI tools - it's been around for years as a standard way to validate data. The schema has two main parts: the name and description at the top (which help Claude understand when to use the tool), and the actual schema that describes the function's arguments.

The top section contains the tool's name and description, which helps Claude understand when to use it. The bottom section is the actual schema that describes your function's arguments in detail.

Creating a JSON Schema: Step-by-Step

Here's the simplest way to create a JSON schema for any function:

Step 1: Write a Dictionary with Sample Data

Take your function and create a dictionary of all keyword arguments with sample data. For example, if you have a function like this:

def process_data(ids, profile, primary_id, value): pass

Create a dictionary with sample values:

Step 2: Convert to JSON

Convert your Python dictionary to proper JSON format. The main difference is changing Python's True to JSON's true.

Step 3: Use an Online Converter

Search for "JSON to JSON Schema converter" and use one of the many free online tools. Paste your JSON data and let it generate the schema automatically.

The tool will analyze your sample data and create a proper schema structure. Remove any $schema declarations from the output - you don't need them.

Step 4: Add Descriptions

The most important step is adding detailed descriptions to each property. These descriptions help Claude understand exactly what each argument does and how to use it.

Writing Good Descriptions

When writing descriptions for your tools and properties, follow these best practices:
• Explain what the tool does, when to use it, and what it returns
• Aim for 3-4 sentences in your tool description
• Provide super detailed descriptions for each property
• If you're stuck, paste your function into Claude and ask it to write descriptions for you

Here's an example of a well-described tool schema:

Notice how the description clearly explains what the weather tool does, when to use it, what data it returns, and provides specific examples of valid location formats.


> *(See full lesson at course URL)*

#### 62. Handling tool use responses

## Handling tool use responses
When Claude decides to use a tool, it returns a special response structure that requires careful handling. Understanding this response format and implementing proper conversation management is crucial for building robust tool-enabled applications.

## Tool Choice Configuration
Before diving into responses, it's worth understanding how to control when Claude uses tools. The [code: : toolChoice] "parameter gives you three options:"
- : **auto** "- Claude decides whether to use a tool (default behavior)"
- : **any** "- Claude must use a tool but can choose which one"
- : **specific tool** "- Force Claude to use a particular tool by name"
The third option is especially useful for testing when you want to ensure Claude calls a specific function.

## Multi-Part Message Structure
"When Claude wants to use a tool, it returns an assistant message with multiple content parts instead of just text:"
"The response contains two parts:"
- : **Text Part** "- Human-readable explanation like \"I can help you find out the current time. Let me find that information for you\""
- : **ToolUse Part** "- Structured data telling you which tool to run and with what arguments"

## Understanding the ToolUse Part
"The ToolUse part contains three key pieces of information:"
- : **toolUseId** "- A unique identifier you'll need when sending back the tool result"
- : **name** "- The exact tool name from your JSON schema that Claude wants to call"
- : **input** "- A dictionary of arguments Claude wants to pass to your tool function"

## Conversation Flow with Tools
"Tool usage follows a specific conversation pattern that requires maintaining complete message history:"
"When you receive a tool use request, you need to:"
- : Extract the tool information from the ToolUse part
- : Run your actual tool function
- : Send back a ToolResult message along with the complete conversation history
- : Include the original user message and the assistant's tool use message in your next request

## Updating Helper Functions

> *(See full lesson at course URL)*

#### 63. Running tool functions

## Running tool functions
When Claude responds with a tool use request, your server needs to actually run the requested tool and send the results back. This step involves extracting tool use parts from Claude's response, executing the appropriate functions, and formatting the results properly.

## Handling Multiple Tool Requests
Claude can send multiple tool use parts in a single response. Your code needs to handle this possibility defensively. An assistant message might contain a text part followed by one, two, or even more tool use parts.
"The flow works like this: Claude sends a request with JSON schema, receives a tool use part, then your server runs the tool and sends back a tool result part for Claude to provide a final response."

## Extracting Tool Use Parts
"First, create a function to process all the parts returned from a chat request:" [code: : "def run_tools(parts): tool_requests = [part for part in parts if \"toolUse\" in part] tool_result_parts = [] for tool_request in tool_requests: tool_use_id = tool_request[\"toolUse\"][\"toolUseId\"] tool_name = tool_request[\"toolUse\"][\"name\"] tool_input = tool_request[\"toolUse\"][\"input\"]"]
This comprehension filters the parts list to only include dictionaries that contain a "toolUse" key, ignoring text parts.

## Running the Actual Tools
"Create a helper function to execute the requested tool:" [code: : "def run_tool(tool_name, tool_input): if tool_name == \"get_current_datetime\": return get_current_datetime(**tool_input) else: raise Exception(f\"Unknown tool name: {tool_name}\")"]
The key detail here is using [code: : "**tool_input"] to splat the dictionary of arguments into your tool function. Claude always returns arguments as a dictionary object, so you need to unpack it properly.

## Creating Tool Result Parts
"After running a tool, you need to format the response as a tool result part:"
"Tool result parts require three key properties:"
- : **toolUseId** "- Must match the original tool use part's ID"
- : **content** "- The output from your tool, serialized as a string"
- : **status** "- Either \"success\" or \"error\""

## Understanding Tool Use IDs
"The tool use ID system becomes important when Claude requests multiple tools in parallel. For example, if Claude wants to run a calculator tool twice:"
Each tool use gets a unique ID (like "ab3" and "po9"), and your tool results must include the matching IDs so Claude knows which result corresponds to which request.

## Error Handling

> *(See full lesson at course URL)*

#### 64. Sending tool results

## Sending tool results
Now we're at the final step of the tool use workflow. After running our tools and getting the results, we need to send everything back to Claude so it can provide a complete response to the user.
"The process is straightforward: take all the tool result parts we generated, package them into a user message, and send the entire conversation history back to Claude along with the original tool schemas."

## Adding the Assistant Message
First, we need to make sure our conversation history is complete. After Claude's initial response with the tool use request, we need to add that response to our message history using [code: : add_assistant_message()] .
"This ensures we have the complete conversation flow: user question → assistant tool request → tool results → final assistant response."

## Running Tools and Creating Tool Results
The [code: : run_tools()] "function processes all the tool use requests from Claude's response and creates properly formatted tool result parts. Each tool result includes:"
- : The tool use ID (matching the original request)
- : The actual output from running the tool
- : A status indicating success or error
The function handles both successful tool executions and errors gracefully, wrapping everything in the correct JSON structure that Claude expects.

## Adding Tool Results to the Conversation
Once we have our tool results, we add them to the conversation using [code: : add_user_message()] ":" [code: : add_user_message(messages, run_tools(parts))]
This creates a user message containing all the tool result parts. The conversation now has the complete back-and-forth needed for Claude to provide a final response.

## Final Call to Claude
"The last step is sending everything back to Claude. This requires two important elements:"
- : The complete message history (user → assistant → user)
- : The original tool schemas

```
: text, parts = chat(messages, tools=[get_current_datetime_schema])
```
Including the tool schemas is crucial. Without them, Claude would be confused about the tool references in the conversation history and wouldn't understand what [code: : get_current_datetime] actually does.

## Success
"When everything works correctly, Claude receives the tool results and can provide a complete, informed response. In our example, Claude successfully retrieved the current time and formatted it in a natural response: \"The current date and time is 2025-04-03, 12:54:00.\""

> *(See full lesson at course URL)*

#### 65. Multi-Turn conversations with tools

## Multi-Turn conversations with tools
Building multi-turn conversations with tool use requires handling different response types from Claude. When Claude responds, it might need to use a tool, or it might provide a direct answer. Your code needs to handle both scenarios gracefully.

## The Problem with Simple Tool Integration
If you just add tool results to every conversation, you'll run into issues. When Claude answers a simple question like "What is 1+1?", it doesn't need any tools. But if your code always tries to process tool results, you'll end up adding empty messages to your conversation history.
The solution is to check the [code: : stop_reason] that comes back with every Claude response. This tells you why Claude stopped generating - whether it finished naturally or because it wants to use a tool.

## Stop Reasons
"Claude can stop for several reasons:"
- :

```
: "\"tool_use\""
```
- :

```
: "\"end_turn\""
```
- :

```
: "\"max_tokens\""
```
- :

```
: "\"stop_sequence\""
```

## Improving the Chat Function
"First, update your chat function to return more information. Instead of just returning text and parts separately, return a dictionary with everything you need:" [code: : "def chat(messages, tools=None, system=None, **kwargs): # ... existing code ... return { \"parts\": parts, \"stop_reason\": response[\"stopReason\"], \"text\": \"\\n\".join([p[\"text\"] for p in parts if \"text\" in p]) }"]
This approach extracts all text content from the response parts, which is more robust than assuming the first part is always text.

## Building a Conversation Loop
"Create a function that handles the full conversation flow:" [code: : "def run_conversation(messages): while True: result = chat(messages, tools=[get_current_datetime_schema]) add_assistant_message(messages, result[\"parts\"]) print(result[\"text\"]) if result[\"stop_reason\"] != \"tool_use\": break tool_result_parts = run_tools(result[\"parts\"]) add_user_message(messages, tool_result_parts) return messages"]
"This loop continues until Claude stops for a reason other than tool use. Each iteration:"
- : Sends the current messages to Claude
- : Adds Claude's response to the message history
- : Checks if Claude wants to use a tool
- : If so, runs the tools and adds results back to the conversation
- : If not, exits the loop

## Testing the Implementation

> *(See full lesson at course URL)*

#### 66. Adding multiple tools

## Adding multiple tools
"Now that we have one tool working, it's time to add the remaining two tools to complete our project:" [code: : add_duration_to_datetime] and [code: : set_reminder] ". The good news is that once you have the foundation in place, adding new tools is straightforward.

## Pre-built Functions and Schemas
"To save time, the implementations for both additional functions are already provided, along with their JSON schema specifications. You can find these in the earlier code cells:"
- : **add_duration_to_datetime** "- Handles date arithmetic for various time units"
- : **set_reminder** "- Creates reminders (currently just prints output, but could be extended to integrate with actual reminder systems)"
Each function comes with a corresponding JSON schema that defines the expected parameters and their types.

## Adding Tools to the Conversation
The first step is to include the new tool schemas in your conversation function. In the [code: : run_conversation] "function, add the additional schemas to the tools array:" [code: : tools=[ get_current_datetime_schema, add_duration_to_datetime_schema, set_reminder_schema ]]

## Wiring Up the Tool Functions
Next, you need to update the [code: : run_tool] "function to handle the new tool names. Add two additional conditional branches:" [code: : "def run_tool(tool_name, tool_input): if tool_name == \"get_current_datetime\": return get_current_datetime(**tool_input) elif tool_name == \"set_reminder\": return set_reminder(**tool_input) elif tool_name == \"add_duration_to_datetime\": return add_duration_to_datetime(**tool_input) else: raise Exception(f\"Unknown tool name: {tool_name}\")"]

## Testing the Complete System
"With all tools connected, you can now test complex workflows that require multiple tool calls. For example, asking Claude to \"Set a reminder to go to the doctor. The appointment is in 100 days\" will trigger a sequence of operations:"
- : Get today's date using

```
: get_current_datetime
```
- : Add 100 days to that date using

```
: add_duration_to_datetime
```
- : Create the reminder using

```
: set_reminder
```
Claude automatically breaks down the request into logical steps and explains its plan before executing each tool call. The output shows the complete workflow, including the calculated future date and confirmation of the reminder being set.

## Key Takeaway

> *(See full lesson at course URL)*

#### 67. Batch tool use

## Batch tool use
Claude can natively run multiple tools at the same time, but some versions don't take advantage of this as much as you might wish. You can greatly increase the chances of Claude making multiple tool calls in a single message by implementing a batch tool.
When Claude sends back tool use parts in a message, there can be more than one tool request in a single response. For example, if you ask "What is March 12th, 2025 + 50 days? Also, what is March 12th, 2025 + 100 days?", Claude could theoretically send back two separate tool use parts - one for each calculation. These operations are completely parallelizable since they don't depend on each other.
However, Claude doesn't always try to parallelize tool calls as much as you'd expect. Instead of making both calls simultaneously, it often makes them sequentially, which is less efficient.

## How the Batch Tool Works
The batch tool is implemented just like any other tool - you need a tool specification and a function to handle when it gets called. The key idea is to create a tool that can invoke multiple other tools simultaneously.
"Here's the basic structure of the batch tool specification:" [code: : "{ \"name\": \"batch_tool\", \"description\": \"Invoke multiple other tool calls simultaneously\", \"input_schema\": { \"type\": \"object\", \"properties\": { \"invocations\": { \"type\": \"array\", \"description\": \"The tool calls to invoke\", \"items\": { \"type\": \"object\", \"properties\": { \"name\": { \"type\": \"string\", \"description\": \"The name of the tool to invoke\" }, \"arguments\": { \"type\": \"string\", \"description\": \"The arguments to the tool, encoded as a JSON string\" } }, \"required\": [\"name\", \"arguments\"] } } }, \"required\": [\"invocations\"] } }"]
The tool takes a list of invocations, where each invocation contains the name of a tool to call and its arguments (encoded as a JSON string).

## Implementation
"The batch tool implementation involves two main functions:"

## The run_batch Function

```
: "def run_batch(tool_input): batch_output = [] for invocation in tool_input[\"invocations\"]: tool_name = invocation[\"name\"] args = json.loads(invocation[\"arguments\"]) tool_output = run_tool(tool_name, args) batch_output.append({\"tool_name\": tool_name, \"output\": tool_output}) return batch_output"
```

> *(See full lesson at course URL)*

#### 68. Structured data with tools

## Structured data with tools
Earlier in this course, we covered how to get structured output from Claude using message pre-fills and stop sequences. While that approach works well and is easy to set up, we can get more reliable output using tools. This method is more complex to implement, but it provides better consistency when extracting structured data like JSON.

## Why Learn Both Approaches?
"You might wonder why we didn't just start with tools if they're more reliable. The answer is simple: tools require significantly more setup and complexity. Having both techniques available gives you flexibility - sometimes you'll want the quick prompt-based approach, other times you'll need the reliability that tools provide."

## How Tool-Based Structured Output Works
"The core concept is straightforward: instead of asking Claude to format its response as JSON, you create a tool whose input parameters match the exact structure of data you want to extract. Claude then \"calls\" this tool with the extracted data as arguments."
"Here's the process:"
- : Write a JSON schema that describes the structure of data you want
- : Create a tool with that schema as its input specification
- : Send your data and the tool schema to Claude
- : Force Claude to use the tool with the

```
: toolChoice
```
- : Extract the structured data from the tool call arguments
"The flow looks like this: your server sends a prompt asking Claude to analyze data and call a specific tool. Claude responds with a tool use message containing the extracted JSON data. At that point, you simply take the data and end the conversation - no follow-up needed."

## Controlling Tool Usage
When using tools for structured output, you want to guarantee that Claude uses your extraction tool. The [code: : toolChoice] "parameter gives you three options:"
- :

```
: "{\"toolChoice\": {\"auto\": {}}}"
```
- :

```
: "{\"toolChoice\": {\"any\": {}}}"
```
- :

```
: "{\"toolChoice\": {\"tool\": {\"name\": \"tool-name\"}}}"
```
For structured output, you'll almost always want the third option to ensure Claude uses your extraction tool.

## Practical Example

> *(See full lesson at course URL)*

#### 69. Flexible tool extraction

## Flexible tool extraction
Writing detailed JSON schemas for structured data extraction can be a real pain point when working with AI tools. There's a clever workaround that lets you specify your desired data structure directly in your prompt instead of creating complex schemas.

## The Flexible Schema Approach
Instead of writing a detailed schema for every data extraction task, you can create one generic tool called [code: : to_json] that accepts any object structure. The key is setting the input schema to allow additional properties, then specifying your exact requirements in the prompt itself.
This approach removes a major pain point - constantly writing and managing large JSON schemas. The results won't be quite as good as a dedicated schema, but you'll still get high-quality JSON output with much less setup work.

## How It Works
"The process is straightforward:"
- : Create a single flexible schema that accepts any object structure
- : In your prompt, specify exactly what data structure you want
- : Tell Claude to call the

```
: to_json
```
- : Use

```
: tool_choice
```

## Setting Up the Prompt
"When writing your prompt, be very explicit about the structure you want. Here's an example of how to structure your request:" [code: : "Analyze the article below and extract key data. Then call the to_json tool. <article_text> {result[\"text\"]} </article_text> When you call to_json, pass in the following structure: {{ \"title\": str # title of the article, \"author\": str # author of the article, \"topics\": List[str] # List of topics mentioned in the article }}"]

## Making the API Call
"The API call uses the flexible schema and forces tool usage:" [code: : flexible_result = chat(messages, tools=[to_json_schema], tool_choice="to_json")]

## Easy Structure Changes
"The real advantage becomes clear when you need to modify your data structure. Instead of rewriting an entire schema, you simply update your prompt. Want to add a field for the number of topics? Just add one line:" [code: : "\"num_topics\": int # Number of topics mentioned"]
That's it - no schema modifications needed.

## When to Use Each Approach
"The flexible schema approach works great for:"
- : Rapid prototyping and experimentation
- : Simple data extraction tasks
- : Situations where you frequently change data requirements
"Stick with dedicated schemas for:"
- : Critical production data extraction tasks
- : Complex nested data structures
- : When you need the highest possible accuracy

> *(See full lesson at course URL)*

#### 70. The text editor tool

## The text editor tool
The Text Editor Tool is Claude's built-in capability that gives it file system access and text editing abilities. Unlike other tools where you write both the schema and implementation, Claude already knows how to request text editor operations - you just need to handle those requests.

## What the Text Editor Tool Does
"This tool gives Claude the ability to work with files and directories like a software engineer would:"
- : View file or directory contents
- : View specific ranges of lines in a file
- : Replace text in files
- : Create new files
- : Insert text at specific line numbers
- : Undo recent edits

## How It Works
The Text Editor Tool is different from custom tools because only the JSON schema is built into Claude. You still need to provide the actual implementation.
When you create custom tools, you write both sides - the schema that tells Claude about the tool, and the function that actually does the work. With the Text Editor Tool, Claude already has the schema, but you must write functions to handle Claude's requests to view, edit, or create files.

## Setting Up the Tool
"To use the Text Editor Tool, you need to provide specific tool names that vary by Claude version:" [code: : "# For Claude 3.7 text_editor = \"text_editor_20250124\" # For Claude 3.5 text_editor = \"text_editor_20241022\""]
You'll also need to modify your chat function to accept the text editor parameter and include it in the model configuration.

## Tool Commands
"When Claude wants to use the text editor, it sends back tool use requests with specific commands:"
"Your implementation needs to handle all five commands. Here's the basic structure for processing these requests:" [code: : "def run_tool(tool_name, tool_input): if tool_name == \"str_replace_editor\": command = tool_input.get(\"command\", \"\") if command == \"view\": path = tool_input.get(\"path\", \"\") return text_editor_tool.view(path) elif command == \"str_replace\": path = tool_input.get(\"path\", \"\") old_str = tool_input.get(\"old_str\", \"\") new_str = tool_input.get(\"new_str\", \"\") return text_editor_tool.str_replace(path, old_str, new_str) # ... handle other commands"]
"Here's how the tool works in practice. When you ask Claude to \"Write a one sentence description of the code in the ./main.py file\", this happens:"

> *(See full lesson at course URL)*

#### 71. Quiz on tool use

## Quiz on tool use

## Quiz on Tool Use


### 📖 Retrieval Augmented Generation

#### 29. Introducing Retrieval Augmented Generation

Retrieval Augmented Generation (RAG) is a technique that helps you work with large documents by breaking them into smaller pieces and only feeding Claude the most relevant chunks for each question. Instead of overwhelming the model with an entire 800-page financial report, RAG lets you extract just the sections that matter for answering specific queries.

The Problem with Large Documents

When you have a massive document and want to ask Claude specific questions about it, you face a fundamental challenge: how do you get the right information to Claude without hitting limits or degrading performance?

Consider asking "What risk factors does this company have?" about a lengthy financial document. The document contains the answer, but Claude needs access to the relevant content to help you.

Option 1: Include Everything in the Prompt

The straightforward approach is extracting all text from the document and stuffing it into a single prompt:

This method has serious limitations:
• Hard token limits mean very long documents simply won't fit
• Claude becomes less effective with extremely long prompts
• Larger prompts cost more money and take longer to process
• Performance degrades when there's too much information to sift through

Option 2: Break Documents into Chunks

RAG takes a smarter approach by preprocessing documents into manageable pieces, then retrieving only the relevant chunks for each question.

Here's how it works:
• Split the document into smaller chunks (Strategy Outlook, Risk Factors, Balance Sheet, etc.)
• When a user asks a question, analyze what they're looking for
• Find the chunks most relevant to their question
• Include only those relevant chunks in the prompt to Claude

For a question about company risks, the system would identify and retrieve the "Risk Factors" chunk, giving Claude focused, relevant context instead of the entire document.

Benefits of RAG:
• Claude can focus on only the most relevant content
• Scales to very large documents and multiple documents
• Works across document collections, not just single files
• Smaller prompts mean faster processing and lower costs

Challenges with RAG:

RAG introduces complexity that you need to manage:
• Requires a preprocessing step to chunk documents
• Need a search mechanism to find relevant chunks
• Retrieved chunks might not contain all necessary context
• Many different ways to chunk text - which approach works best?


> *(See full lesson at course URL)*

#### 30. Text embeddings

After breaking a document into chunks, the next step in a RAG pipeline is finding which chunks are most relevant to a user's question. This is fundamentally a search problem - you need to look through all your text chunks and identify the ones that relate to what the user is asking about.

The challenge is determining which chunks are "related" to a user's question. This isn't as simple as keyword matching - you need to understand the meaning and context of both the question and the chunks.

The most common solution is semantic search, which uses text embeddings to understand what each piece of text is actually about, rather than just looking for exact word matches.

What Are Text Embeddings?

A text embedding is a numerical representation of the meaning contained in some text. Think of it as converting words and sentences into a format that computers can work with mathematically.

Here's how it works:
• You feed text into an embedding model
• The model outputs a long list of numbers (typically 1024 numbers)
• Each number represents a "score" for some quality of the input text
• The numbers range from -1 to +1

Understanding the Numbers

Each number in an embedding is like a score for some aspect of the text. While we don't know exactly what each position represents, it's helpful to think of them as measuring different qualities.

For example, one number might score "how happy the text is" while another might measure "how much the text talks about oceans." The key point is that we don't actually know what each number represents - the embedding model learns these patterns during training, and they're not human-interpretable.

Generating Embeddings with Code

Creating embeddings is straightforward. Here's the basic process:

def generate_embedding(text, embedding_model_id="amazon.titan-embed-text-v2:0", dimensions=1024, normalize=True):
    request_body = {
        "inputText": text,
        "dimensions": dimensions,
        "normalize": normalize,
    }
    request_json = json.dumps(request_body)
    response = client.invoke_model(
        modelId=embedding_model_id,
        body=request_json,
        accept="application/json",
        contentType="application/json",
    )
    response_body = json.loads(response.get("body").read())
    return response_body["embedding"]

When you run this function on a text chunk, you get back a list of 1024 numbers that represent the semantic meaning of that text.


> *(See full lesson at course URL)*

#### 31. The full RAG flow

Now that we've covered the basics of RAG, text chunking, and embeddings, let's walk through the complete RAG pipeline step by step. This detailed example will show you exactly how all the pieces fit together in a real implementation.

Step 1: Chunk Your Source Text

First, we take our source document and break it into manageable chunks. For this example, we'll use two simple text sections:
• Section 1: Medical Research - "This year saw significant strides in our understanding of XDR-47, a 'bug' we have not seen before."
• Section 2: Software Engineering - "This division dedicated significant effort to studying various infection vectors in our distributed systems"

Step 2: Generate Embeddings

Next, we convert each text chunk into numerical embeddings. To make this concept clear, let's imagine we have a perfect embedding model that always returns exactly two numbers, and we know what each number represents:

In our imaginary model:
• First number: How much the text talks about the medical field
• Second number: How much the text talks about software engineering

So our medical research section gets an embedding of [0.97, 0.34] - very medical, somewhat software-related due to the word "bug". The software engineering section gets [0.30, 0.97] - very software-focused, but "infection vectors" has medical connotations.

Normalization

Before storing these embeddings, they go through a normalization process that scales each vector to have a magnitude of 1.0. This is typically handled automatically by your embedding API, but it's important to understand that it happens.

After normalization, our embeddings become [0.944, 0.331] and [0.295, 0.955]. We can visualize these on a unit circle where each point lies exactly on the circle's edge.

Step 3: Store in Vector Database

The normalized embeddings get stored in a vector database - a specialized database optimized for storing, comparing, and searching through long lists of numbers like our embeddings.

At this point, we pause. All the work so far has been preprocessing that happens ahead of time. Now we wait for a user to submit a query.

Step 4: Process User Query

When a user asks a question like "I'm curious about the company. In particular, what did the software engineering dept do this year?", we run their query through the same embedding model.

This query gets embedded as [0.1, 0.89] - low medical score, high software engineering score. After normalization, it becomes [0.112, 0.993].


> *(See full lesson at course URL)*

#### 76. Quiz on Retrieval Augmented Generation

## Quiz page


### 📖 Features of Claude

#### 32. Extended thinking

Extended thinking is Claude's advanced feature that gives the model time to reason through complex problems before generating a final response. Think of it as Claude's internal monologue - you can see how it approaches your problem step by step.

How Extended Thinking Works

When you enable extended thinking, Claude's response includes two parts instead of one:
• Reasoning Content Part - Claude's internal thinking process
• Text Part - The final response you actually wanted

The reasoning content shows you exactly how Claude breaks down your problem, what it considers, and how it arrives at its final answer. This transparency can be incredibly valuable for understanding and debugging complex tasks.

Trade-offs to Consider

Extended thinking comes with clear benefits and costs:
• Better accuracy on complex tasks
• Higher cost - you pay for all thinking tokens
• Increased latency - thinking takes time

The key decision point is simple: use your evaluations. If you've already optimized your prompt but still aren't getting the accuracy you need, that's when extended thinking becomes worth considering.

The Signature System

One important detail you'll notice immediately is the cryptographic signature attached to reasoning content:

This signature ensures you can't modify the thinking text. If you want to include Claude's previous reasoning in a follow-up conversation, the signature verifies the content hasn't been tampered with. This prevents potential safety issues from modified reasoning text.

Redacted Content

Sometimes Claude's thinking gets flagged by safety systems. When this happens, you'll receive a redactedContent field instead of readable thinking text:

The redacted content is encrypted but still functional - you can pass it back to Claude in future conversations without losing context. It's just not readable to you as a developer.

Implementation

To enable extended thinking, you need to modify your API call with two parameters:

additional_model_fields["thinking"] = { "type": "enabled", "budget_tokens": thinking_budget }

The thinking_budget controls how many tokens Claude can spend on reasoning. The minimum is 1024 tokens, but you might need more for complex problems. Like everything else with Claude, use your evaluations to find the right budget for your use case.

Here's how the updated chat function looks:


> *(See full lesson at course URL)*

#### 33. Image support

Claude's vision capabilities allow you to include images in your messages and ask Claude to analyze, compare, count objects, or perform virtually any visual task you can imagine. This opens up powerful possibilities for applications ranging from document analysis to automated assessments.

Image Handling Basics

When working with images in Claude, you need to understand a few key limitations:
• Up to 20 images across all messages in a single request
• Max size of 3.75MB
• Max height/width of 8000px
Each image counts as a certain number of tokens: tokens = (width px × height px) / 750

To include an image, you add it as another type of message part. For each image you want to send, you include one image part in your user message. The structure looks like this:

with open("image.png", "rb") as f:
    image_bytes = f.read()
add_user_message(messages, [
    { "image": { "format": "png", "source": {"bytes": image_bytes} } },
    {"text": "What do you see in this image?"}
])

Multiple Images

You can send multiple images in a single message by adding multiple image parts. Claude can then analyze relationships between images, compare them, or answer questions that require understanding multiple visual inputs.

Prompting Techniques

The most important thing to understand about Claude's vision capabilities is that all the same prompting engineering techniques apply to images. You can dramatically increase Claude's vision accuracy by providing guidelines, analysis steps, or using one-shot/multi-shot examples.

For example, instead of simply asking "How many marbles are in this image?", you can provide a structured approach:

Analyze this image of marbles and determine the exact count using this methodology:
1. Begin by identifying each unique marble one at a time. Assign each a number as you identify it.
2. Verify your result by counting with a different method. Start from the bottom-left corner and work row by row, from left to right.
What is the exact, verified number of marbles in this image?

Another effective technique is one-shot prompting, where you provide an example image with the correct analysis before asking Claude to analyze your target image.

Real-World Example: Fire Risk Assessments

A practical application of Claude's vision capabilities is automated fire risk assessment for insurance companies. Instead of sending inspectors to each property, companies can use high-resolution satellite imagery and ask Claude to evaluate fire risks.


> *(See full lesson at course URL)*

#### 34. Prompt caching

Prompt caching is a feature that speeds up Claude's responses and reduces the cost of text generation by reusing computational work from previous requests. To understand how this works, let's first look at what normally happens inside Claude during a typical request.

How Claude Normally Processes Requests

When you send a message to Claude, a lot happens behind the scenes before you get a response back. Claude doesn't just immediately start generating text - it first does extensive work on your input message.

Here's what Claude does with your message:
• Tokenize the prompt
• Create embeddings for each token
• Add context based on surrounding text
• Generate output text

All of this preprocessing work happens before Claude generates any actual response. Once Claude finishes processing your request and sends back the response, it throws away all the computational work it just did.

The Problem with Throwing Away Work

This creates an inefficiency when you're having conversations with Claude. Let's say you make a follow-up request that includes the same message from earlier, plus Claude's previous response, plus a new message to continue the conversation.

When Claude sees that original message again, it has to redo all the same computational work it just threw away moments earlier. Claude essentially thinks: "I just processed this exact message and did all this work, then threw it away. Now I have to do it all over again."

How Prompt Caching Solves This

Prompt caching addresses this inefficiency by saving the computational work instead of discarding it. Here's how it works:

When Claude processes your initial request, instead of throwing away all the preprocessing work, it stores that work in a cache. The cache acts like a lookup table that maps specific input messages to their corresponding computational results.

When you make a follow-up request that includes the same content, Claude can check its cache and reuse the previous work instead of starting from scratch.

Key Benefits and Limitations

Prompt caching offers several advantages:
• Requests that use cached content are cheaper and faster to execute
• Initial request will write to the cache
• Follow up requests can read from the cache
• Cache lives for 5 minutes
• Only useful if you're repeatedly sending the same content (but this happens extremely frequently)


> *(See full lesson at course URL)*

#### 77. Quiz on features of Claude

## Quiz page


### 📖 Agents

#### 35. Agents overview



### 📖 Wrap up

#### 36. Course wrap up


#### 37. Implementing the RAG flow

This walkthrough demonstrates the complete RAG (Retrieval-Augmented Generation) implementation using a practical example. We'll build a vector database from scratch and execute all five steps of the RAG workflow using a sample report document.

Setting Up the Vector Database

The implementation uses a custom VectorIndex class that handles storing embeddings and performing similarity searches. This class provides the core functionality we need for our vector database operations.

The Five-Step RAG Implementation

Step 1: Chunk the Text by Section

First, we load and chunk our source document using the same section-based chunking approach from earlier:

with open("./report.md", "r") as f: text = f.read() chunks = chunk_by_section(text)

This breaks our report into logical sections that can be processed independently.

Step 2: Generate Embeddings for Each Chunk

Next, we create embeddings for every chunk using a list comprehension:

embeddings = [generate_embedding(chunk) for chunk in chunks]

This step involves multiple API calls, so it takes some time to complete. Each chunk gets converted into a numerical vector representation.

Step 3: Store Embeddings in the Vector Database

Now we create our vector store and populate it with both embeddings and their associated text:

store = VectorIndex()
for embedding, chunk in zip(embeddings, chunks):
    store.add_vector(embedding, {"content": chunk})

The key insight here is that we store both the embedding and the original text. Just getting back a list of numbers isn't useful - we need the actual text content that corresponds to those embeddings. This metadata allows us to retrieve meaningful results later.

Step 4: Generate User Query Embedding

When a user asks a question, we convert it to the same embedding format:

user_embedding = generate_embedding("What did the software engineering dept do last year?")

This creates a vector representation of the user's question that can be compared against our stored embeddings.

Step 5: Search and Retrieve Relevant Chunks

Finally, we search our vector store to find the most similar content:

results = store.search(user_embedding, 2)
for doc, distance in results:
    print(distance, "\n", doc["content"][0:200], "\n")

This returns the two most relevant chunks along with their cosine distance scores. Lower distances indicate higher similarity.

Understanding the Results


> *(See full lesson at course URL)*

#### 38. BM25 lexical search

When building a RAG pipeline, you'll quickly discover that semantic search alone doesn't always return the best results. Sometimes you need exact keyword matches that semantic search might miss. The solution is to combine semantic search with lexical search using a technique called BM25.

The Problem with Semantic Search Alone

Let's say you're searching for a specific incident ID like "INC-2023-Q4-011" in a document. While this exact term appears multiple times in relevant sections, semantic search might return unrelated sections that seem semantically similar but don't actually contain the specific information you need.

This happens because semantic search focuses on meaning rather than exact text matches. When you need precise keyword matching, you need a different approach.

Hybrid Search Strategy

The solution is to run both semantic and lexical searches in parallel, then merge the results. This gives you the best of both worlds:

Semantic search - Finds conceptually related content using embeddings

Lexical search - Finds exact keyword matches using classic text search

Merged results - Combines both approaches for better overall relevance

How BM25 Works

BM25 (Best Match 25) is a popular algorithm for lexical search in RAG pipelines. Here's how it processes a search query:

The algorithm follows these key steps:

Tokenize the query - Break the user's question into individual terms

Count term frequency - See how often each term appears across all documents

Weight terms by rarity - Terms used less frequently get higher importance scores

Score documents - Find text chunks that contain more instances of the higher-weighted terms

The key insight is that rare terms like "INC-2023-Q4-011" are much more important for search relevance than common words like "a" or "the".

Implementing BM25 Search

Here's how to set up a BM25 search system:

# Create a BM25 store
store = BM25Index()
# Add documents to the store
for chunk in chunks:
    store.add_document({"content": chunk})
# Search the store
results = store.search("What happened with INC-2023-Q4-011?", 3)

The BM25 implementation maintains a similar API to your vector store, with add_document() and search() methods. This consistency makes it easy to use both systems together.

Better Search Results


> *(See full lesson at course URL)*

#### 39. A multi-search RAG pipeline

When you have both semantic search (vector embeddings) and lexical search (BM25) working independently, the next step is combining them into a unified search pipeline. This hybrid approach leverages the strengths of both methods to deliver more accurate results.

Building a Unified Interface

Both search implementations share nearly identical APIs - they both have add_document() and search() methods. This consistency makes it straightforward to wrap them in a single Retriever class that coordinates between the two approaches.

The Retriever acts as a coordinator that:

- Receives a user's question
- Forwards it to both the VectorIndex and BM25Index
- Collects results from both systems
- Merges the results using a ranking algorithm

Reciprocal Rank Fusion

The challenge lies in merging results from different search methods. Each system returns results with different scoring mechanisms, so you can't simply combine scores directly. Instead, we use a technique called Reciprocal Rank Fusion (RRF).

Here's how RRF works with a practical example. Suppose your VectorIndex returns results ranked as: Section 2, Section 7, Section 6. Meanwhile, your BM25Index returns: Section 6, Section 2, Section 7.

To merge these results, you create a combined table showing each text chunk's rank from both systems:

The RRF formula calculates a score for each document:

RRF_score(d) = Σ(1 / (k + rank_i(d)))

Where k is a constant (typically 60, though 1 works well for clearer results) and rank_i(d) is the rank of document d in the i-th ranking system.

For each text chunk, you calculate:

- Section 2: 1.0/(1+1) + 1.0/(1+2) = 0.833
- Section 7: 1.0/(1+2) + 1.0/(1+3) = 0.583
- Section 6: 1.0/(1+3) + 1.0/(1+1) = 0.75

After sorting by score, the final ranking becomes: Section 2 (first), Section 6 (second), Section 7 (third).

Implementation

The Retriever class implementation is straightforward:

class Retriever:
    def __init__(self, *indexes):
        self._indexes = list(indexes)
    
    def add_document(self, document):
        for index in self._indexes:
            index.add_document(document)
    
    def search(self, query_text, k=1, k_rrf=60):
        # Get results from all indexes
        all_results = []
        for idx, results in enumerate(all_results):
            for rank, (doc, _) in enumerate(results):
                # Track document ranks across systems
        # Apply RRF formula
        # Return merged, sorted results


> *(See full lesson at course URL)*

#### 40. Reranking results

The hybrid retrieval approach we've built works well, but there are still some rough edges. When you search for specific terms or use abbreviations, the results might not be perfectly ordered. For example, asking about specific terms might return sections that seem related but aren't the best match.

LLM-Based Re-ranking

Re-ranking adds another post-processing step after merging results from your vector index and BM25 index. The concept is straightforward: take your search results and ask Claude to reorder them based on relevance to the user's question.

Here's how the process works:

- Run your hybrid search (vector + BM25) as usual
- Merge the results like before
- Pass the merged results to a re-ranker function
- The re-ranker sends everything to Claude with a specific prompt
- Claude returns a reordered list of the most relevant documents

System Prompts

The re-ranking prompt is designed to be clear and specific. You provide Claude with the user's question and all the documents that seem relevant, then ask for a simple task: return the most relevant documents in order of decreasing relevance.

A typical prompt structure looks like this:

You are tasked with finding the documents most relevant to a user's question.
<user_question> What happened with INC-2023-Q4-011? </user_question>
Here are documents that may be relevant:
<documents>
  <document>Section 10...</document>
  <document>Section 2...</document>
  <document>Section 7...</document>
  <document>Section 6...</document>
</documents>
Return the 3 most relevant docs, in order of decreasing relevance.

Efficiency Considerations

Asking Claude to return full text chunks would be inefficient - you'd be waiting for Claude to copy large amounts of text. Instead, assign each text chunk a unique ID ahead of time. Then ask Claude to return just those IDs in the correct order.

This approach is much faster because Claude only needs to return a simple list like document IDs instead of copying entire document sections.

Implementation

The re-ranker function gets called automatically by your retriever after the initial hybrid search. Here's the basic structure:

def reranker_fn(docs, query_text, k):
    # Format documents with IDs
    joined_docs = "\n".join([
        f"<document><document_id>{doc['id']}</document_id>"
        f"<document_content>{doc['content']}</document_content></document>"
        for doc in docs
    ])
    # Create prompt with user question and documents

> *(See full lesson at course URL)*

#### 41. Contextual retrieval

Contextual retrieval is a technique that improves RAG pipeline accuracy by solving a fundamental problem: when you split a document into chunks, each chunk loses its connection to the broader document context.

The basic idea is simple. After chunking your source document, you ask Claude to add context to each chunk before storing it in your retriever database. This pre-processing step helps situate each chunk within the larger document.

How It Works

For each text chunk, you send both the chunk and the original source document to Claude with a prompt asking for a short snippet to situate the chunk within the overall document. Claude generates context that describes the chunk's relationship to the larger document.

You then combine this generated context with the original chunk text to create a contextualized chunk that gets stored in your vector and BM25 indexes.

Handling Large Documents

If your source document is too large to fit in a single prompt, you can provide a reduced set of context instead of the entire document. Include a few chunks from the start of the document and chunks immediately preceding the target chunk.

Implementation Example

The contextual retrieval function takes a text chunk and source text, sends them to Claude with a prompt asking for succinct context, then combines the generated context with the original chunk.

When processing document chunks, loop through each one and generate contextualized versions by building context from start chunks and preceding chunks, then adding to the retriever.

Expected Results

The generated context provides valuable information about document structure and relationships. This additional context helps the retrieval system better understand not just what each chunk contains, but how it fits into the larger document structure and relates to other sections. Contextual retrieval becomes increasingly valuable as documents become more complex with intricate cross-references.

#### 42. Quiz on Retrieval Augmented Generation

This is a quiz lesson with 5 questions covering Retrieval Augmented Generation topics including vector databases, embeddings, hybrid search, BM25, and contextual retrieval.

#### 43. PDF support

Claude can read and analyze PDF documents just as easily as it handles images. This capability opens up powerful possibilities for document analysis, summarization, and question-answering workflows.

Setting Up PDF Processing

To work with PDFs, you'll need to make a few key changes to the standard message structure. The process is similar to image handling, but with some important differences in the document specification.

First, read your PDF file as binary data: with open("./earth.pdf", "rb") as f: file_bytes = f.read()

Document Message Structure

The message structure for PDFs differs from images in several ways. Instead of an image object, you'll use a document object with these required fields:

Use "document" instead of "image", set "format": "pdf", include a "name" field with the filename without extension, and the "source" contains the file bytes.

When Claude analyzes the PDF, it processes the entire document content and provides a comprehensive response, successfully summarizing multi-page documents with complex layouts, images, and structured information.

What Claude Can Do with PDFs

Claude can handle various PDF processing tasks: extract and summarize key information, answer specific questions about document content, analyze document structure and formatting, process multi-page documents efficiently, and work with PDFs containing both text and images.

The PDF processing capability becomes even more powerful when combined with other features like citations, which allow Claude to reference specific parts of the document in its responses.

#### 44. Citations

When working with PDFs in Claude, one of the biggest challenges is trust. Users often have to take it on faith that the AI is correctly interpreting the document contents. Claude's citations feature directly addresses this problem by showing exactly where information comes from in your source documents.

Enabling Citations

To enable citations in your PDF processing, you need to add a single parameter to your document configuration: set citations.enabled to True in the document dictionary. This tells Claude to track where it finds information and include citation data in its response.

Understanding Citation Responses

When citations are enabled, Claude's response structure changes significantly. Instead of just returning text, you get multiple parts: the regular response content, and citations content that includes detailed information about where Claude found supporting evidence for each statement, including the specific document, page numbers, and even the exact text that influenced its response.

Why Citations Matter

Citations provide several key benefits for PDF-based applications:

- Verification: Users can check Claude's work by going back to the source
- Confidence: Knowing where information comes from builds trust in AI responses
- Transparency: The AI's reasoning process becomes visible and auditable
- Accuracy: Citations encourage more careful information extraction

This feature is particularly valuable in professional, academic, or research contexts where accuracy and source attribution are critical.

#### 45. Rules of prompt caching

Prompt caching in Claude works by storing the computational work done on messages so it can be reused in follow-up requests. This makes subsequent requests both cheaper and faster to execute, but only when you're repeatedly sending the same content.

The process follows a two-phase pattern: the initial request writes to the cache, and follow-up requests can read from it. The cache only lives for 5 minutes, so this feature is most useful when you're sending the same content repeatedly within a short timeframe.

Cache Points

Prompt caching is not enabled automatically - you need to manually add cache point message parts to control what gets cached. Cache points tell Claude to cache all the work done for everything before that point in your message.

Here is how you add a cache point to a user message: set cachePoint.type to default. The key rule is that work done for everything before the cache point will be cached, but anything after the cache point will not be stored in the cache.

How Cache Points Work

When you make an initial request with a cache point, Claude processes all the content and stores the work done up to that cache point. On follow-up requests, if the content before the cache point is identical, Claude reads the previously processed work from cache instead of reprocessing it.

The cache will only be used if the content before the cache point is completely identical. Even small changes like adding text to the beginning of your prompt will prevent cache usage, forcing Claude to process everything from scratch.

Caching Across Messages

Cache points can span multiple messages and even include assistant messages. This means you can cache entire conversation histories up to a certain point.

Minimum Content Length

Content must be at least 1024 tokens long to be cached. This is the sum of all messages and parts you are trying to cache before the cache point.

Cache Point Locations

Cache points are not restricted to user messages. You can add them to system prompts and tool definitions, which are actually the most common caching opportunities. These are the most valuable caching opportunities because system prompts and tool lists rarely change between requests.

#### 46. Prompt caching in action

This lesson demonstrates prompt caching in action through a Jupyter notebook. The lesson covers practical implementation of prompt caching patterns including: setting up cache points in user messages, adding cache points to system prompts, caching tool definitions across requests, and optimizing for the 1024 token minimum content length requirement. The notebook provides hands-on examples of the caching patterns covered in the Rules of Prompt Caching lesson.

#### 47. Introducing MCP

Model Context Protocol (MCP) is a communication layer that provides Claude with context and tools without requiring you to write a bunch of tedious integration code. Instead of building every tool function yourself, MCP shifts that burden to specialized servers that handle the heavy lifting.

When you first encounter MCP, you will see diagrams showing the basic architecture: an MCP Client (your server) connects to MCP Servers that contain tools, prompts, and resources. Each MCP Server acts as an interface to outside services like GitHub, AWS, or databases.

The Problem MCP Solves

Let's say you are building a chat interface where users can ask Claude about their GitHub data - questions like What open pull requests are there across all my repositories? To handle this without MCP, you would need to create tools for every GitHub operation you want to support.

GitHub has massive functionality - repositories, pull requests, issues, projects, and much more. Building a complete GitHub integration means authoring an incredible number of tool schemas and functions.

How MCP Works

MCP shifts the burden of tool definitions and execution from your server to dedicated MCP Servers. Instead of writing all those GitHub tools yourself, you connect to a GitHub MCP Server that already has them implemented.

The MCP Server acts as a wrapper around the outside service, providing pre-built tools that Claude can use. You get access to all that GitHub functionality without writing any of the integration code yourself.

Common Questions

Who authors MCP Servers? Anyone can create an MCP Server implementation. Often, service providers themselves will make their own official implementations. For example, AWS might release an official MCP Server with tools for their various services.

How is this different from calling APIs directly? When you call a service API directly, you still have to write the tool schemas and function implementations yourself. MCP Servers provide those tool schemas and functions already defined for you, saving you development time.

Is not MCP just the same as tool use? This is a common misconception. MCP Servers and tool use are complementary but different concepts. Tool use is about Claude calling functions to accomplish tasks. MCP is about who provides those functions - instead of you writing them, someone else has already implemented them in an MCP Server.


> *(See full lesson at course URL)*

#### 48. MCP clients

The MCP client serves as the communication bridge between your server and MCP servers. Think of it as your access point to all the tools that an MCP server provides. When you need to use external functionality, the client handles all the message passing and protocol details for you.

Transport Agnostic Communication

One of MCP key strengths is being transport agnostic - the client and server can talk to each other using different communication methods. The most common setup runs both the MCP client and server on the same machine, where they communicate through standard input/output. But you are not limited to that approach - MCP clients and servers can also connect over HTTP, WebSockets, and various other network protocols.

Message Types

Once connected, the client and server exchange specific message types defined in the MCP specification. The main ones you will work with are:

- ListToolsRequest/ListToolsResult: The client asks the server what tools do you provide? and gets back a complete list of available functionality.
- CallToolRequest/CallToolResult: The client tells the server run this specific tool with these arguments and receives the execution results.

Complete Flow Example

Here is how all the pieces work together in a real scenario. Let us say a user asks What repositories do I have? - here is the complete communication flow:

The process starts when a user submits their question to your server. But before your server can ask Claude for help, it needs to know what tools are available.

Your server asks the MCP client for a list of tools. The client sends a ListToolsRequest to the MCP server and gets back a ListToolsResult with all available tools.

Now your server has everything needed to make the initial request to Claude: the user question plus the list of available tools.

Claude analyzes the tools and decides it needs to call one to answer the question. It responds with a tool use request.

Your server recognizes that Claude wants to run a tool, but your server does not execute tools directly anymore - that is the MCP server job. So it asks the MCP client to run the tool with Claude specified arguments.

The MCP client sends a CallToolRequest to the MCP server, which then makes the actual request to GitHub to fetch the user repositories. GitHub responds with the repository data, which the MCP server wraps in a CallToolResult and sends back to the MCP client.


> *(See full lesson at course URL)*

#### 49. Project setup

We are going to build our own CLI-based chatbot to better understand how MCP clients and servers work together. This hands-on project will give you practical experience with both sides of the MCP architecture.

What We Are Building

Our chatbot will be a command-line interface that allows users to chat with a set of documents. Here is what the system will include:

- A CLI-based chatbot interface
- Document reading and editing capabilities for Claude
- Document mention functionality using @doc_name syntax
- Command execution with /command_name syntax
- A collection of fake documents stored in memory

System Architecture

The project consists of three main components working together:

- Our MCP Client: Handles user interaction and chat interface
- Our MCP Server: Provides tools for document operations
- Document Storage: In-memory collection of various file types

The MCP server will implement two core tools:

- Tool to read document contents
- Tool to update document contents

All documents (PDFs, spreadsheets, text files, markdown files) will be stored in memory rather than on disk, keeping the project simple and focused on MCP concepts.

Important Architecture Note

In real-world projects, you typically implement either an MCP client or an MCP server - not both. You might build an MCP server to distribute a service to other developers, or build an MCP client that connects to existing third-party MCP servers.

Our project implements both components in a single codebase purely for educational purposes, so you can see how clients and servers interact with each other.

#### 50. Defining tools with MCP

Building an MCP server becomes much simpler when you use the official Python SDK. Instead of manually writing complex JSON schemas for tools, you can define them with decorators and let the SDK handle the heavy lifting.

In this example, we are creating an MCP server that manages document operations. The server will have two main tools: one to read document contents and another to update them. All documents exist in memory as a simple dictionary where keys are document IDs and values are the content strings.

MCP Python SDK Benefits

The MCP project provides official SDKs for building servers and clients across multiple programming languages. Using the Python SDK offers several advantages:

- Creates MCP servers with minimal boilerplate code
- Automatically generates JSON schemas from Python function signatures
- Simplifies tool definition through decorators
- Handles type validation and error handling

The @mcp.tool decorator, combined with type hints and field descriptions, automatically creates the proper tool schema that Claude can understand and use.

Setting Up the Server

The basic server setup requires just a few lines to import FastMCP and set up the server with an in-memory document store.

Implementing the Read Tool

The first tool allows Claude to read document contents by providing a document ID. The tool definition includes a clear name that describes the action, a description explaining what the tool does, typed parameters with field descriptions, and error handling for invalid document IDs.

Implementing the Edit Tool

The second tool performs simple find-and-replace operations on document content. This tool takes three parameters: the document ID, the text to find, and the replacement text. The implementation uses Python built-in string replace method for simplicity.

Key Implementation Details

When defining tools with the MCP SDK, remember these important points:

- Import Field from pydantic to add parameter descriptions
- Use type hints to specify parameter types
- Include error handling for edge cases
- Write clear, descriptive tool names and descriptions
- The SDK automatically converts your function signature into the proper JSON schema

The MCP Python SDK dramatically reduces the complexity of creating tools compared to manually writing JSON schemas.

#### 51. The server inspector

When building MCP servers, you need a way to test your functionality without connecting to a full application. The Python MCP SDK includes a built-in browser-based inspector that lets you debug and test your server in real-time.

Starting the Inspector

First, make sure your Python environment is activated. Then run the inspector with: mcp dev mcp_server.py

This starts a development server and gives you a local URL (typically on port 6277) to access the inspector in your browser.

Using the Inspector Interface

The MCP inspector is actively being developed, so the interface may look different by the time you use it. However, the core functionality remains consistent.

When you first open the inspector, you will see a Connect button on the left side. Click this to start your MCP server and load your tools.

Testing Your Tools

Once connected, look for a navigation bar with sections like Resources, Prompts, and Tools. Click on the Tools section to see your available tools.

Click List Tools to see all the tools your server provides. When you select a specific tool, the right panel updates to show a form where you can test that tool.

Running Tool Tests

For example, to test a document reading tool, select the read_doc_contents tool, enter a document ID, and click Run Tool. The inspector will execute your tool and show the results, including success status and any returned data.

Testing Document Editing

You can also test more complex tools like document editing. Switch to the edit_document tool, fill in the document ID, old text to replace, and new text, then run the tool to see if it succeeds. Use the read tool again to verify the changes were applied.

Development Workflow

The inspector shows a history of your tool calls on the left side, making it easy to track what you have tested and repeat previous operations. This creates an efficient development loop where you can make changes to your server code, restart the inspector, test your tools immediately, and verify the results.

This inspector tool becomes essential as you build more complex MCP servers. It eliminates the need to wire up your server to a full application just to test basic functionality, making development much faster and more reliable.

#### 52. Implementing a client

Now that we have our MCP server working, it is time to build the client side. The client is what allows our application to communicate with the MCP server and access its functionality.

Understanding the Client Architecture

Before diving into the code, let us clarify an important point about MCP projects. Normally, you would implement either an MCP client or an MCP server - not both. We are building both in this project just so you can see how they work together.

The MCP client consists of two main components working together:

- MCP Client: A custom class we create to make using the session easier
- Client Session: The actual connection to the server (part of the MCP Python SDK)

The client session handles the low-level communication but requires careful resource cleanup when your program shuts down. That is why we wrap it in our own class - to manage that cleanup automatically.

How the Client Fits Into Our Application

The client plays a crucial role in two key moments - getting available tools and executing tools when Claude requests them.

Implementing Core Client Functions

We need to implement two essential functions: list_tools and call_tool.

For list_tools, we need to connect to our session and request the available tools. For call_tool, we pass the tool name and input parameters to the server.

Testing the Client

The client file includes a simple test harness at the bottom. You can run it directly to verify everything works: uv run mcp_client.py

This will connect to your MCP server and print out the available tools.

Important Schema Differences

Here is a gotcha you need to know about: MCP tool definitions do not exactly match what Claude expects. The MCP spec has its own format for tool schemas, which is slightly different from what Bedrock requires. Do not worry - there is already code in the project that handles this conversion automatically. The to_bedrock_tools function translates MCP tool definitions into the format Claude understands.

Testing with Claude

Now that both the server and client are working, you can test the complete flow. Try running your main application and asking Claude to read a document. Claude will receive the list of available tools from your client, decide to use the read_doc_contents tool, your client will execute that tool on the MCP server, and Claude will receive the document contents and respond.


> *(See full lesson at course URL)*

#### 53. Defining resources

Resources in MCP servers allow you to expose data to clients, similar to GET request handlers in a typical HTTP server. They are perfect for scenarios where you need to fetch information rather than perform actions.

Understanding Resources

Think of resources as read-only endpoints that can return any type of data - strings, JSON, binary files, etc. You set a mime_type to give the client a hint about what kind of data you are returning.

Resources work by exposing data through URIs. When a client needs data, it sends a ReadResourceRequest with the specific URI, and your server responds with the requested information.

Two Types of Resources

There are two main types of resources you can create:

- Direct Resources: Have static URIs that do not contain any parameters (like docs://documents)
- Templated Resources: Include parameters in their URIs (like docs://documents/{doc_id})

For templated resources, the Python SDK automatically parses parameters from the URI and passes them as keyword arguments to your function.

Implementing Resources

Creating resources is straightforward using the @mcp.resource() decorator. Direct resources return a list of available documents, while templated resources fetch specific documents by ID.

The MCP Python SDK automatically serializes whatever you return. You do not need to manually convert data to JSON strings - just return the appropriate Python data structure.

Testing Your Resources

You can test resources using the MCP Inspector tool. Start your server and navigate to the web interface. The inspector separates direct resources from templated ones. Direct resources appear in the main Resources section, while templated resources show up under Resource Templates.

Practical Use Cases

Resources are ideal for providing autocomplete data like document lists, fetching file contents or database records, exposing configuration data, and serving any read-only information your client needs.

The key advantage is that resources allow clients to proactively fetch data without relying on tools or complex interactions. This makes them perfect for features like document mentions, where you want to automatically inject content into prompts based on user references.

#### 54. Accessing resources

Resources in MCP allow your server to expose data that can be directly included in prompts, rather than requiring tool calls to access information. This creates a more efficient way to provide context to AI models like Claude.

Understanding the Resource Flow

When a user types something like what is in the @ in your application, the system needs to fetch a list of available resources for autocomplete. The MCP client sends a ReadResourceRequest to the server, which responds with a list of document names that can be referenced.

Implementing Resource Reading

The core functionality happens in the read_resource method of your MCP client. This method takes a URI parameter that identifies which resource to fetch from the server.

First, add the necessary imports to handle JSON parsing and URL validation. The main implementation makes a request to the MCP server and processes the response.

Handling Different Content Types

Resources can return different types of content, so you need to check the MIME type to handle the response appropriately. JSON resources are properly parsed, while plain text resources are returned as-is.

Testing the Implementation

Once implemented, you can test the resource functionality by running your CLI application. When you type @ followed by a resource name, the system will show available resources in an autocomplete list, allow you to select a resource using arrow keys and space, and include the resource content directly in the prompt sent to Claude.

This means Claude receives the document content immediately without needing to make additional tool calls, making the interaction much more efficient.

Key Benefits

Resources provide several advantages over tools for accessing static information:

- Content is included directly in prompts, reducing latency
- No additional API calls needed during conversation
- Better user experience with autocomplete functionality
- Cleaner separation between static data and dynamic operations

Resources work best for relatively static information that you want to make easily accessible to AI models, such as documentation, reports, or reference materials.

#### 55. Defining prompts

MCP servers can define prompts - pre-written, high-quality instructions that clients can use instead of writing their own prompts from scratch. Think of prompts as carefully crafted templates that give better results than what users might write on their own.

Why Use Prompts?

Let us say you want Claude to reformat a document into markdown. You could just ask and it would work fine. But you would probably get much better results with a thoroughly tested, detailed prompt that covers edge cases and gives specific formatting instructions.

The idea is simple: as MCP server developers, we can spend time crafting and testing really good prompts, then make them available to anyone using our server. Users get better results without having to become prompt engineering experts themselves.

Defining a Prompt

Prompts use a similar decorator pattern to tools and resources. The function returns a list of messages that can be sent directly to Claude. This lets you build complex prompts with multiple user and assistant messages if needed.

Building the Format Prompt

For our document server, we create a prompt that reformats documents into markdown. The prompt takes a document ID as input, uses the read_doc_contents tool to get the document, reformats it with proper markdown syntax, and saves the changes back to the document.

Testing the Prompt

Once you have defined your prompt, you can test it using the MCP Inspector. Navigate to the Prompts tab, select your prompt, and provide the required parameters. The inspector will show you the generated messages that would be sent to Claude.

Key Benefits

Prompts provide several key benefits:

- Quality Control: Server authors can test and refine prompts before users see them
- Consistency: Everyone gets the same high-quality prompt instead of improvising
- Specialization: Prompts can be tailored to your server specific domain and capabilities
- Reusability: Multiple client applications can use the same well-crafted prompts

Prompts are particularly valuable when your MCP server has a specific focus area like document management, data analysis, or code generation. You can provide users with battle-tested prompts that leverage your server tools effectively.

#### 56. Prompts in the client

The final step in building our MCP client is implementing prompt functionality. This allows us to list all available prompts from the server and retrieve specific prompts with variables interpolated into them.

Implementing List Prompts

The list_prompts method is straightforward. We call the session list prompts method and return the prompts.

Getting Individual Prompts

The get_prompt method is more interesting because it handles argument interpolation. When we request a specific prompt, we pass arguments that get injected into the prompt function. For example, if our server has a format prompt that expects a doc_id parameter, that value gets passed through and interpolated into the actual prompt text.

The method returns messages that form a conversation ready to be fed directly into Claude.

Testing Prompts in Action

When you run the client and type a forward slash, you will see available prompts as commands. Selecting a prompt like format will prompt you to choose from available documents. The system then takes the prompt with the document ID interpolated, feeds it directly to Claude as a user message, Claude receives both the instructions and the document ID, Claude uses available tools to fetch the document content, and Claude responds with the reformatted result.

How Prompts Work

Prompts define a set of user and assistant messages that can be used by the client. These prompts should be high quality, well-tested, and relevant to the overall purpose of your MCP server.

The workflow is: Write and evaluate a prompt relevant to your MCP server purpose, define the prompt inside your MCP server using the @mcp.prompt decorator, your client can request that prompt at any time, when requesting the prompt provide arguments that get passed as keyword arguments to the prompt function, and the function uses those arguments to customize the prompt content.

This system creates reusable, parameterized prompts that can be shared across different clients and use cases, making your MCP server more versatile and powerful.

#### 57. MCP review

Now that we have built our MCP server, let us recap the three core server primitives and understand when to use each one. The key insight is that each primitive is controlled by a different part of your application stack.

Tools: Model-Controlled

Tools are controlled entirely by Claude. The AI model decides when to call these functions, and the results are used directly by Claude to accomplish tasks.

Use tools when you want to give Claude additional capabilities. For example, if you ask Claude to calculate the square root of 3 using JavaScript, Claude will automatically decide to use a JavaScript execution tool to provide the answer. The decision to use the tool was 100% model-controlled - Claude recognized it needed to execute code and chose the appropriate tool without any prompting from the application or user.

Resources: App-Controlled

Resources are controlled by your application code. Your app decides when to fetch resource data and how to use it, typically for UI purposes or to add context to conversations.

Use resources when you need to get data into your app. Common examples include populating autocomplete options in your UI, fetching documents to display in a file picker, and adding context to messages before sending them to Claude. In Claude web interface, the Add from Google Drive feature demonstrates this perfectly. The application fetches a list of available documents and displays them in the UI, then injects the selected document content into the chat context.

Prompts: User-Controlled

Prompts are controlled by users. They decide when to trigger these predefined workflows through direct actions like clicking buttons, selecting menu options, or using slash commands.

Use prompts when you want to implement predefined workflows that users can easily access. In Claude interface, you will see workflow buttons below the chat input that let users quickly start common tasks like writing, learning, or coding.

Choosing the Right Primitive

When building your MCP server, think about who needs to control each piece of functionality:

- Need to extend Claude capabilities? Use tools
- Need to get data into your app UI? Use resources
- Need to offer users predefined workflows? Use prompts

These are high-level guidelines to help you choose the right primitive based on your specific use case. Each serves a different part of the application stack - tools serve the model, resources serve the app, and prompts serve the users.

#### 58. Claude Code setup

This lesson covers the setup process for Claude Code. The lesson includes a video demonstration and hands-on exercises to get started with Claude Code in your development environment. Topics covered include installing Claude Code CLI, configuring authentication, setting up your working directory, and basic command usage.

#### 59. Claude Code in action

Claude Code is not just a tool for writing code - it is designed to be your coding partner throughout an entire project lifecycle. From initial setup to deployment and maintenance, Claude can help with every step of software development.

The /init Command

When starting with a new project, the /init command is your first step. Claude Code will scan your codebase, noting project structure, dependencies, commands, and coding patterns. The findings get summarized in a CLAUDE.md file that Claude automatically reads in future conversations.

You can have multiple CLAUDE.md files for different scopes:

- Project: checked into git, shared between engineers
- Local: not checked into git, your particular notes to Claude
- User: used across all projects

When running /init, you can add special directions for areas you want Claude to focus on. You can also use the # shortcut to add quick notes that get appended to your CLAUDE.md file.

Common Workflows

Claude works best as an effort multiplier. The more context and structure you provide, the better results you will get.

Planning-First Workflow: This three-step approach works well for complex features:
1. Feed context into Claude - find files relevant to your feature and ask Claude to read them
2. Tell Claude to plan a solution - describe what you want built, but specifically ask Claude not to write code yet
3. Ask Claude to implement the solution - once you have a solid plan, Claude can write code based on the context and planning it already completed

Test-Driven Development Workflow: This approach requires more upfront effort but dramatically increases Claude effectiveness:
1. Feed context into Claude - share relevant files for your feature
2. Ask Claude to think of test cases - tell Claude specifically not to write any code yet
3. Ask Claude to implement those tests
4. Select only the tests that look relevant to your feature
5. Ask Claude to write code that passes the tests - Claude will iterate on a solution until the tests pass

Practical Tips

Claude can handle routine development tasks beyond just writing code. You can ask it to set up project environments and install dependencies, stage and commit changes with descriptive commit messages, and run test suites and interpret results. Clear conversation history with /clear to reset context.


> *(See full lesson at course URL)*

#### 60. Enhancements with MCP servers

Claude Code has an MCP client built right into it, which means you can connect MCP servers to dramatically expand its functionality. This opens up some really powerful possibilities for customizing your development workflow.

How MCP Integration Works

The Model Context Protocol allows Claude Code to connect to external services through MCP servers. Each server can provide tools, prompts, and resources that extend what Claude can do.

In this example, we will connect Claude Code to a custom MCP server that provides a document conversion tool. This will let Claude read and convert PDF and Word documents to markdown format.

Adding an MCP Server to Claude Code

Setting up an MCP server is straightforward. First, stop any running Claude Code session, then use the MCP add command: claude mcp add documents uv run main.py

This command takes two arguments: the server name (can be anything you want - documents in this case) and the command to start your MCP server.

After adding the server, restart Claude Code and it will automatically connect to your MCP server.

Testing the Integration

Once connected, Claude can use the tools provided by your MCP server. In our example, we can ask Claude to convert document files to markdown format, and it will automatically use the document conversion tool we created.

Popular MCP Servers for Development

There are many existing MCP servers that can enhance your development workflow:

- sentry-mcp: Automatically discover and fix bugs logged in Sentry
- playwright-mcp: Gives Claude browser automation capabilities for testing and troubleshooting
- figma-context-mcp: Exposes Figma designs to Claude
- mcp-atlassian: Allows Claude to access Confluence and Jira
- firecrawl-mcp-server: Adds web scraping capabilities to Claude
- slack-mcp: Allows Claude to post messages or reply to specific threads

Building Your Custom Workflow

The real power comes from combining multiple MCP servers that match your specific development needs. For example, you might set up a Sentry server to fetch production error details, a Jira server to read ticket requirements, a Slack server to notify your team when work is complete, and custom servers for your specific tools and processes.

This flexibility makes Claude Code incredibly adaptable to different development environments and workflows.

#### 61. Parallelizing Claude Code

Running multiple instances of Claude Code in parallel is one of the biggest productivity gains you can achieve. Since Claude is lightweight, you can easily spin up several copies, assign each a different task, and have them work simultaneously. This effectively gives you a team of virtual software engineers working on your project.

The Challenge: File Conflicts

The main problem with parallel instances is that they might try to modify the same files at the same time. This can lead to conflicting or invalid code since each instance is not aware of what the others are doing.

The solution is to give each Claude instance its own separate workspace. Each instance works with its own copy of your project, makes changes in isolation, and then merges those changes back into your main project.

Git Worktrees

Git worktrees are perfect for this workflow. If your project is already managed by Git, you can use worktrees immediately. They are like an extension of Git branching functionality that lets you create complete copies of your project in separate directories on your machine.

Each worktree corresponds to a separate branch. You can have one folder for feature A and another for feature B, each containing a complete copy of your codebase. Then you run separate Claude Code instances in each worktree, working in total isolation.

Once each Claude instance finishes its feature, you commit the work and merge it back into your main branch, just like merging any normal Git branch.

Automating Worktree Creation

This might sound complicated to manage, but you can delegate the entire workflow to Claude Code itself. You can write a prompt that asks Claude to create a new git worktree in a specific folder, symlink dependencies that are not tracked by Git, and launch a new VS Code instance in that directory.

Custom Commands

Rather than copying and pasting long prompts every time, you can create custom slash commands in Claude Code. Add a .md file to .claude/commands to create a custom command.

The custom command can reference $ARGUMENTS, which gets replaced with whatever arguments you pass to your command. For example, /project:create_worktree feature_a creates a worktree named feature_a.

Parallel Development in Action

Each Claude instance works on its assigned task: update document tests, add logging, add note-taking tools, add a subtract tool.

Merging Changes


> *(See full lesson at course URL)*


### 📖 Model Context Protocol

#### 78. Quiz on Model Context Protocol

## Quiz page


### 📖 Final assessment

#### 79. Final assessment quiz

## Quiz page


---

*Summary generated from course content at https://anthropic.skilljar.com/claude-in-amazon-bedrock*
