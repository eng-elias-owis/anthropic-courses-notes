# Building with the Claude API - Course Summary

**Course URL:** https://anthropic.skilljar.com/claude-with-the-anthropic-api

---

## 🎯 Course Overview

Comprehensive course on integrating the Claude API into applications. Covers API access, prompt engineering, tool use, retrieval augmented generation (RAG), Model Context Protocol (MCP), and building AI agents.

---

## 📚 Table of Contents


**Introduction**
  📌 Welcome to the course
  📌 Overview of Claude models

**Accessing Claude with the API**
  📌 Accessing the API
  📌 Getting an API key
  📌 Making a request
  📌 Multi-Turn conversations
  📌 Chat exercise
  📌 System prompts
  📌 System prompts exercise
  📌 Temperature
  ✅ Course satisfaction survey
  📌 Response streaming
  📌 Structured data
  📌 Structured data exercise
  ✅ Quiz on accessing Claude with the API

**Prompt evaluation**
  📌 Prompt evaluation
  📌 A typical eval workflow
  📌 Generating test datasets
  📌 Running the eval
  📌 Model based grading
  📌 Code based grading
  📌 Exercise on prompt evals
  ✅ Quiz on prompt evaluation

**Prompt engineering techniques**
  📌 Prompt engineering
  📌 Being clear and direct
  📌 Being specific
  📌 Structure with XML tags
  📌 Providing examples
  📌 Exercise on prompting
  ✅ Quiz on prompt engineering techniques

**Tool use with Claude**
  📌 Introducing tool use
  📌 Project overview
  📌 Tool functions
  📌 Tool schemas
  📌 Handling message blocks
  📌 Sending tool results
  📌 Multi-turn conversations with tools
  📌 Implementing multiple turns
  📌 Using multiple tools
  📌 Fine grained tool calling
  📌 The text edit tool
  📌 The web search tool
  ✅ Quiz on tool use with Claude

**RAG and Agentic Search**
  📌 Introducing Retrieval Augmented Generation
  📌 Text chunking strategies
  📌 Text embeddings
  📌 The full RAG flow
  📌 Implementing the RAG flow
  📌 BM25 lexical search
  📌 A Multi-Index RAG pipeline

**Features of Claude**
  📌 Extended thinking
  📌 Image support
  📌 PDF support
  📌 Citations
  📌 Prompt caching
  📌 Rules of prompt caching
  📌 Prompt caching in action
  📌 Code execution and the Files API
  ✅ Quiz on features of Claude

**Model Context Protocol**
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
  ✅ Quiz on Model Context Protocol

**Anthropic apps - Claude Code and computer use**
  📌 Anthropic apps
  📌 Claude Code setup
  📌 Claude Code in action
  📌 Enhancements with MCP servers

**Agents and workflows**
  📌 Agents and workflows
  📌 Parallelization workflows
  📌 Chaining workflows
  📌 Routing workflows
  📌 Agents and tools
  📌 Environment inspection
  📌 Workflows vs agents
  ✅ Quiz on Agents and Workflows

**Final assessment**
  📌 Final Assessment

**Wrapping up!**
  📌 Course Wrap Up

---

## 📖 Lesson Content


### 📖 Introduction

#### 1. Welcome to the course


#### 2. Overview of Claude models



### 📖 Accessing Claude with the API

#### 3. Accessing the API


#### 4. Getting an API key

In the next video we will be making a request to the Anthropic API. To do so, you will need a secret API key. This guide will walk you through the process of creating an API key.

Step One: Navigate to the Anthropic API Console
In your browser, navigate to https://console.anthropic.com/ and log in to your Anthropic account. You'll then see a page like this:

Step Two: Click the 'Get API Keys' button
This button can be found towards the top right of the main dashboard page.

Step Three: Click the 'Create Key' button
At the top right of the page, find the 'Create Key' button and click it.

Step Four: Enter a workspace and name for your key
Create the key in workspace 'Default' and enter a name for your key. This name is used to help you identify the keys you generate. Let's use a name of 'Anthropic Course'.

Step Five: Copy the Key
Your API key will then be displayed in a pop up window. Copy this key and hold onto it - we will use it in the next video. This key will only be displayed once, so make sure you copy it!
If you accidentally close the window, delete the old key and generate it again.

#### 5. Making a request

Making your first request to the Anthropic API is straightforward once you understand the basic setup and structure. This guide walks through the essential steps to get Claude responding to your prompts using Python.

Setting Up Your Environment
Before making any API calls, you need to install the required packages and configure your API key securely.
First, install the necessary dependencies in your Jupyter notebook:
%pip install anthropic python-dotenv
Next, create a .env file in the same directory as your notebook to store your API key securely:
ANTHROPIC_API_KEY="your-api-key-here"
This approach keeps your API key out of your code and prevents accidentally committing it to version control. Always add .env to your .gitignore file.
Load the environment variables and create your API client:
from dotenv import load_dotenv load_dotenv() from anthropic import Anthropic client = Anthropic() model = "claude-sonnet-4-0"

The Create Function
The core of making API requests is the client.messages.create() function. This function requires three key parameters:
model - The name of the Claude model you want to use
max_tokens - A safety limit on response length (not a target)
messages - The conversation history you're sending to Claude
The max_tokens parameter acts as a safety mechanism. If you set it to 1000, Claude will stop generating after 1000 tokens even if it has more to say. Claude doesn't try to reach this limit - it just writes what it thinks is appropriate and stops if it hits the maximum.

Understanding Messages
Messages represent the conversation between you and Claude, similar to a chat application. There are two types of messages:
User messages - Content you want to send to Claude (written by humans)
Assistant messages - Responses that Claude has generated
Each message is a dictionary with a role (either "user" or "assistant") and content (the actual text).

Making Your First Request
Here's a complete example of making a request to Claude:
message = client.messages.create( model=model, max_tokens=1000, messages=[ { "role": "user", "content": "What is quantum computing? Answer in one sentence" } ] )
When you run this code, Claude will process your request and return a response object containing the generated text along with metadata about the request.

Extracting the Response
The response object contains a lot of information, but you usually just want the generated text. Access it using:
message.content[0].text

> *(See full lesson at course URL)*

#### 6. Multi-Turn conversations

When working with the Anthropic API and Claude, there's a crucial concept you need to understand: Claude doesn't store any of your conversation history. Each request you make is completely independent, with no memory of previous exchanges.

This means if you want to have a multi-turn conversation where Claude remembers context from earlier messages, you need to handle the conversation state yourself.

The Problem with Stateless Conversations
Let's say you ask Claude "What is quantum computing?" and get a good response. Then you follow up with "Write another sentence" - Claude has no idea what you're referring to. It will write a sentence about something completely random because it has no memory of the quantum computing discussion.

How Multi-Turn Conversations Work
To maintain conversation context, you need to do two things:
1. Manually maintain a list of all messages in your code
2. Send the complete message history with every request

Here's the flow that actually works:
1. Send your initial user message to Claude
2. Take Claude's response and add it to your message list as an assistant message
3. Add your follow-up question as another user message
4. Send the entire conversation history to Claude

Building Helper Functions
To make conversation management easier, you can create three helper functions:
def add_user_message(messages, text): user_message = {"role": "user", "content": text} messages.append(user_message) def add_assistant_message(messages, text): assistant_message = {"role": "assistant", "content": text} messages.append(assistant_message) def chat(messages): message = client.messages.create( model=model, max_tokens=1000, messages=messages, ) return message.content[0].text

Putting It All Together
Here's how you use these functions to maintain a conversation:
# Start with an empty message list messages = [] # Add the initial user question add_user_message(messages, "Define quantum computing in one sentence") # Get Claude's response answer = chat(messages) # Add Claude's response to the conversation history add_assistant_message(messages, answer) # Add a follow-up question add_user_message(messages, "Write another sentence") # Get the follow-up response with full context final_answer = chat(messages)

Now Claude will understand that "Write another sentence" refers to expanding on the quantum computing definition, because you've provided the complete conversation context.


> *(See full lesson at course URL)*

#### 7. Chat exercise


#### 8. System prompts

System prompts are a powerful way to customize how Claude responds to user input. Instead of getting generic answers, you can shape Claude's tone, style, and approach to match your specific use case.

Why System Prompts Matter
Consider building a math tutor chatbot. When a student asks "How do I solve 5x + 2 = 3 for x?", you want Claude to act like a real tutor, not just spit out the answer. A good math tutor should:
- Initially give hints rather than complete solutions
- Patiently walk students through problems step by step
- Show solutions for similar problems as examples

You definitely don't want Claude to:
- Immediately give direct answers
- Tell students to just use a calculator

How System Prompts Work
System prompts provide Claude with guidance on how to respond. You define them as plain strings and pass them into the create function call. The key benefits are:
- System prompts provide Claude guidance on how to respond
- Claude will try to respond in the same way someone in the specified role would respond
- Helps keep Claude on task

Here's the basic structure:
system_prompt = """ You are a patient math tutor. Do not directly answer a student's questions. Guide them to a solution step by step. """ client.messages.create( model=model, messages=messages, max_tokens=1000, system=system_prompt )

Seeing the Difference
Without a system prompt, Claude gives a complete step-by-step solution immediately. This might be helpful, but it doesn't encourage the student to think through the problem themselves.

With the math tutor system prompt, Claude's response changes dramatically. Instead of providing the full solution, Claude asks guiding questions like "What do you think would be a good first step to isolate x? Consider what operation we might need to perform on both sides to start moving terms around."

Building a Flexible Chat Function
Rather than hard-coding system prompts, you can make your chat function more reusable by accepting system prompts as parameters:
def chat(messages, system=None): params = { "model": model, "max_tokens": 1000, "messages": messages, } if system: params["system"] = system message = client.messages.create(**params) return message.content[0].text
This approach handles an important detail: Claude's API doesn't accept system=None, so you need to conditionally include the system parameter only when it's provided.

Now you can call your chat function with or without a system prompt:

> *(See full lesson at course URL)*

#### 9. System prompts exercise


#### 10. Temperature

Temperature is a powerful parameter that controls how predictable or creative Claude's responses will be. Understanding how to use it effectively can dramatically improve your AI applications.

How Claude Generates Text
Before diving into temperature, it helps to understand Claude's text generation process. When you send Claude a prompt like "What do you think?", it goes through three key steps:
- Tokenization - Breaking your input into smaller chunks
- Prediction - Calculating probabilities for possible next words
- Sampling - Choosing a token based on those probabilities

In this example, Claude might assign a 30% probability to "about", 20% to "would", 10% to "of", and so on. The model then selects one token and repeats this entire process to build complete sentences.

What Temperature Does
Temperature is a decimal value between 0 and 1 that directly influences these selection probabilities. It's like adjusting the "creativity dial" on Claude's responses.

At low temperatures (near 0), Claude becomes very deterministic - it almost always picks the highest probability token. At high temperatures (near 1), Claude distributes probability more evenly across options, leading to more varied and creative outputs.

Interactive Temperature Demo
You can see temperature in action with Claude's interactive demo. Watch how the probability distribution changes as you adjust the temperature slider:

At temperature 0.0, "about" gets 100% probability - completely deterministic. At temperature 1.0, probabilities spread more evenly across all possible tokens, introducing randomness and creativity.

Choosing the Right Temperature
Different tasks call for different temperature ranges:

Low Temperature (0.0 - 0.3):
- Factual responses
- Coding assistance
- Data extraction
- Content moderation

Medium Temperature (0.4 - 0.7):
- Summarization
- Educational content
- Problem-solving
- Creative writing with constraints

High Temperature (0.8 - 1.0):
- Brainstorming
- Creative writing
- Marketing content
- Joke generation

Implementing Temperature in Code
Adding temperature support to your chat function is straightforward. Here's how to modify your existing function:
def chat(messages, system=None, temperature=1.0): params = { "model": model, "max_tokens": 1000, "messages": messages, "temperature": temperature } if system: params["system"] = system message = client.messages.create(**params) return message.content[0].text


> *(See full lesson at course URL)*

#### 11. Course satisfaction survey

video only

#### 12. Response streaming

When building chat applications with Claude, there's a significant user experience challenge: responses can take 10-30 seconds to generate, leaving users staring at a loading spinner. The solution is response streaming, which lets users see text appear chunk by chunk as Claude generates it, creating a much more responsive feel.

The Problem with Standard Responses
In a typical chat setup, your server sends a user message to Claude and waits for the complete response before sending anything back to the client. This creates an awkward delay where users have no feedback that anything is happening.

How Streaming Works
With streaming enabled, Claude immediately sends back an initial response indicating it has received your request and is starting to generate text. Then you receive a series of events, each containing a small piece of the overall response.

Your server can forward these text chunks to your client application as they arrive, allowing users to see the response building up word by word. All of these events are part of a single request to Claude.

Understanding Stream Events
When you enable streaming, Claude sends back several types of events:
- MessageStart - A new message is being sent
- ContentBlockStart - Start of a new block containing text, tool use, or other content
- ContentBlockDelta - Chunks of the actual generated text
- ContentBlockStop - The current content block has been completed
- MessageDelta - The current message is complete
- MessageStop - End of information about the current message

The ContentBlockDelta events contain the actual generated text that you'll want to display to users.

Basic Streaming Implementation
To enable streaming, add stream=True to your messages.create call:
messages = [] add_user_message(messages, "Write a 1 sentence description of a fake database") stream = client.messages.create( model=model, max_tokens=1000, messages=messages, stream=True ) for event in stream: print(event)

Simplified Text Streaming
Rather than manually parsing events, you can use the SDK's simplified streaming interface that extracts just the text content:
with client.messages.stream( model=model, max_tokens=1000, messages=messages ) as stream: for text in stream.text_stream: print(text, end="")

This approach automatically filters out everything except the actual text content, which is usually what you need for displaying responses to users.

Getting the Complete Message

> *(See full lesson at course URL)*

#### 13. Structured data

When you need Claude to generate structured data like JSON, Python code, or bulleted lists, you'll often run into a common problem: Claude wants to be helpful and add explanatory text around your content. While this is usually great, sometimes you need just the raw data with nothing else.

Consider building a web app that generates AWS EventBridge rules. Users enter a description, click generate, and expect to see clean JSON they can immediately copy and use. If Claude returns the JSON wrapped in markdown code blocks with explanatory text, users can't simply copy the entire response - they have to manually select just the JSON portion.

The Problem with Default Responses
By default, when you ask Claude to generate JSON, you might get something like this:
```json
{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": {
    "state": ["running"]
  }
}
```
This rule captures EC2 instance state changes when instances start running.

The JSON is correct, but it's wrapped in markdown formatting and includes explanatory text. For a web app where users need to copy the raw JSON, this creates friction in the user experience.

The Solution: Assistant Message Prefilling + Stop Sequences
You can combine assistant message prefilling with stop sequences to get exactly the content you want. Here's how it works:
messages = [] add_user_message(messages, "Generate a very short event bridge rule as json") add_assistant_message(messages, "```json") text = chat(messages, stop_sequences=["```"])

This technique works by:
- The user message tells Claude what to generate
- The prefilled assistant message makes Claude think it already started a markdown code block
- Claude continues by writing just the JSON content
- When Claude tries to close the code block with "```", the stop sequence immediately ends generation

The result is clean JSON with no extra formatting:
{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": {
    "state": ["running"]
  }
}

Processing the Response
You might notice some extra newline characters in the response. These are easy to handle:
import json
# Clean up and parse the JSON
clean_json = json.loads(text.strip())

Beyond JSON
This technique isn't limited to JSON generation. Use it anytime you need structured data without commentary:
- Python code snippets
- Bulleted lists
- CSV data
- Any formatted content where you want just the content, not explanations


> *(See full lesson at course URL)*

#### 14. Structured data exercise


#### 15. Quiz on accessing Claude with the API



### 📖 Prompt evaluation

#### 16. Prompt evaluation

When working with Claude, writing a good prompt is just the beginning. To build reliable AI applications, you need to understand two critical concepts: prompt engineering and prompt evaluation. Prompt engineering gives you techniques for writing better prompts, while prompt evaluation helps you measure how well those prompts actually work.

Prompt Engineering vs Prompt Evaluation
Prompt engineering is your toolkit for crafting effective prompts. It includes techniques like:
- Multishot prompting
- Structuring with XML tags
- Many other best practices

These techniques help Claude understand exactly what you're asking for and how you want it to respond.

Prompt evaluation takes a different approach. Instead of focusing on how to write prompts, it's about measuring their effectiveness through automated testing. You can:
- Test against expected answers
- Compare different versions of the same prompt
- Review outputs for errors

Three Paths After Writing a Prompt
Once you've drafted a prompt, you typically face three options for what to do next:

Option 1: Test the prompt once and decide it's good enough. This carries a significant risk of breaking in production when users provide unexpected inputs.

Option 2: Test the prompt a few times and tweak it to handle a corner case or two. While better than option 1, users will often provide very unexpected outputs that you haven't considered.

Option 3: Run the prompt through an evaluation pipeline to score it, then iterate on the prompt based on objective metrics. This approach requires more work and cost, but gives you much more confidence in your prompt's reliability.

Why Most Engineers Fall Into Testing Traps
Options 1 and 2 are common traps that all engineers fall into, myself included. It's natural to write a prompt for a serious application and not test it thoroughly enough. We tend to underestimate how many edge cases real users will encounter.

The reality is that when you deploy a prompt to production, users will interact with it in ways you never anticipated. What seemed like a solid prompt during your limited testing can quickly break down when faced with the full variety of real-world inputs.

The Evaluation-First Approach
Option 3 represents a more systematic approach to prompt development. By running your prompt through an evaluation pipeline, you get objective metrics about its performance across a broader range of test cases. This data-driven approach lets you:

> *(See full lesson at course URL)*

#### 17. A typical eval workflow

A typical prompt evaluation workflow follows five key steps that help you systematically improve your prompts through objective measurement. While there are many different ways to assemble these workflows and various open source and paid tools available, understanding the core process helps you start small and scale up as needed.

Step 1: Draft a Prompt
Start by writing an initial prompt that you want to improve. For this example, we'll use a simple prompt:
prompt = f""" Please answer the user's question: {question} """
This basic prompt will serve as our baseline for testing and improvement.

Step 2: Create an Eval Dataset
Your evaluation dataset contains sample inputs that represent the types of questions or requests your prompt will handle in production. The dataset should include questions that will be interpolated into your prompt template.

For this example, our dataset includes three questions:
- "What's 2+2?"
- "How do I make oatmeal?"
- "How far away is the Moon?"

In real-world evaluations, you might have tens, hundreds, or even thousands of records. You can assemble these datasets by hand or use Claude to generate them for you.

Step 3: Feed Through Claude
Take each question from your dataset and merge it with your prompt template to create complete prompts. Then send each one to Claude to get responses.

For example, the first question becomes:
Please answer the user's question: What's 2+2?

Claude might respond with "2 + 2 = 4" for the math question, provide oatmeal cooking instructions for the second question, and give the distance to the Moon for the third.

Step 4: Feed Through a Grader
The grader evaluates the quality of Claude's responses by examining both the original question and Claude's answer. This step provides objective scoring, typically on a scale from 1 to 10, where 10 represents a perfect answer and lower scores indicate room for improvement.

In our example, the grader might assign:
- Math question: 10 (perfect answer)
- Oatmeal question: 4 (needs improvement)
- Moon question: 9 (very good answer)

The average score across all questions gives you an objective measurement: (10 + 4 + 9) ÷ 3 = 7.66

Step 5: Change Prompt and Repeat
Now that you have a baseline score, you can modify your prompt and run the entire process again to see if your changes improve performance.

For example, you might add more guidance to your prompt:
prompt = f""" Please answer the user's question: {question} Answer the question with ample detail """


> *(See full lesson at course URL)*

#### 18. Generating test datasets

Building a custom prompt evaluation workflow starts with creating a solid prompt and then generating test data to see how well it performs. Let's walk through setting up an evaluation system for a prompt that helps users write AWS-specific code.

Setting Up the Goal
Our prompt needs to assist users in writing three specific types of output for AWS use cases:
- Python code
- JSON configuration files
- Regular expressions

The key requirement is that when a user requests help with a task, we return clean output in one of these formats without any extra explanations, headers, or footers.

Here's our starting prompt (version 1):
prompt = f""" Please provide a solution to the following task: {task} """

Creating an Evaluation Dataset
An evaluation dataset contains inputs that we'll feed into our prompt. For each combination of prompt and input, we'll run the prompt and analyze the results.

Our dataset will be an array of JSON objects, where each object contains a "task" property describing what we want Claude to accomplish. We can either create this dataset by hand or generate it automatically using Claude.

Since we're generating test data, this is a perfect opportunity to use a faster model like Haiku instead of the full Claude model.

Generating Test Data with Code
Let's create a function that automatically generates our test dataset. First, we'll need our helper functions for working with Claude:
def add_user_message(messages, text): user_message = {"role": "user", "content": text} messages.append(user_message) def add_assistant_message(messages, text): assistant_message = {"role": "assistant", "content": text} messages.append(assistant_message) def chat(messages, system=None, temperature=1.0, stop_sequences=[]): params = { "model": model, "max_tokens": 1000, "messages": messages, "temperature": temperature } if system: params["system"] = system if stop_sequences: params["stop_sequences"] = stop_sequences response = client.messages.create(**params) return response.content[0].text

Now we'll create our dataset generation function:

> *(See full lesson at course URL)*

#### 19. Running the eval

Now that we have our evaluation dataset ready, it's time to build the core evaluation pipeline. This involves taking each test case, merging it with our prompt, feeding it to Claude, and then grading the results.

The evaluation process follows a clear workflow: we take our dataset of test cases, combine each one with our prompt template, send it to Claude for processing, and then evaluate the output using a grader system.

Building the Core Functions
The evaluation pipeline consists of three main functions, each with a specific responsibility. Let's start with the simplest one - the function that handles individual prompts.

The run_prompt Function
This function takes a test case and merges it with our prompt template:
def run_prompt(test_case): """Merges the prompt and test case input, then returns the result""" prompt = f""" Please solve the following task: {test_case["task"]} """ messages = [] add_user_message(messages, prompt) output = chat(messages) return output

Right now, we're keeping the prompt extremely simple. We're not including any formatting instructions, so Claude will likely return more verbose output than we need. We'll refine this later as we iterate on our prompt design.

The run_test_case Function
This function orchestrates running a single test case and grading the result:
def run_test_case(test_case): """Calls run_prompt, then grades the result""" output = run_prompt(test_case) # TODO - Grading score = 10 return { "output": output, "test_case": test_case, "score": score }

For now, we're using a hardcoded score of 10. The grading logic is where we'll spend significant time in upcoming sections, but this placeholder lets us test the overall pipeline.

The run_eval Function
This function coordinates the entire evaluation process:
def run_eval(dataset): """Loads the dataset and calls run_test_case with each case""" results = [] for test_case in dataset: result = run_test_case(test_case) results.append(result) return results

This function processes every test case in our dataset and collects all the results into a single list.

Running the Evaluation
To execute our evaluation pipeline, we load our dataset and run it through our functions:
with open("dataset.json", "r") as f: dataset = json.load(f) results = run_eval(dataset)

The first time you run this, expect it to take some time - even with Claude Haiku, it can take around 30 seconds to process a full dataset. We'll cover optimization techniques later.

Examining the Results

> *(See full lesson at course URL)*

#### 20. Model based grading

When building prompt evaluation workflows, grading systems provide objective signals about output quality. A grader takes model output and returns some kind of measurable feedback - typically a number between 1 and 10, where 10 represents high quality and 1 represents poor quality.

Types of Graders
There are three main approaches to grading model outputs:
- Code graders - Programmatically evaluate outputs using custom logic
- Model graders - Use another AI model to assess the quality
- Human graders - Have people manually review and score outputs

Code Graders
Code graders let you implement any programmatic check you can imagine. Common uses include:
- Checking output length
- Verifying output does/doesn't have certain words
- Syntax validation for JSON, Python, or regex
- Readability scores

The only requirement is that your code returns some usable signal - usually a number between 1 and 10.

Model Graders
Model graders feed your original output into another API call for evaluation. This approach offers tremendous flexibility for assessing:
- Response quality
- Quality of instruction following
- Completeness
- Helpfulness
- Safety

Human Graders
Human graders provide the most flexibility but are time-consuming and tedious. They're useful for evaluating:
- General response quality
- Comprehensiveness
- Depth
- Conciseness
- Relevance

Defining Evaluation Criteria
Before implementing any grader, you need clear evaluation criteria. For a code generation prompt, you might focus on:
- Format - Should return only Python, JSON, or Regex without explanation
- Valid Syntax - Produced code should have valid syntax
- Task Following - Response should directly address the user's task with accurate code

The first two criteria work well with code graders, while task following is better suited for model graders due to their flexibility.

Implementing a Model Grader
Here's how to build a model grader function:

> *(See full lesson at course URL)*

#### 21. Code based grading

When evaluating AI models that generate code, you need more than just checking if the response makes sense. You also need to verify that the generated code actually has valid syntax and follows the correct format. This is where code-based grading comes in.

How Code Grading Works
Code grading validates two key aspects of AI-generated responses:
- Format - The response should return only the requested code type (Python, JSON, or Regex) without explanations
- Valid Syntax - The generated code should actually parse correctly as the intended language
- Task Following - The response should directly address what was asked and be accurate

The first two criteria are handled by the code grader, while task following is evaluated by the model grader. Together, they provide a comprehensive evaluation.

Syntax Validation Functions
To check if generated code has valid syntax, you can create three helper functions that attempt to parse the output:
def validate_json(text): try: json.loads(text.strip()) return 10 except json.JSONDecodeError: return 0 def validate_python(text): try: ast.parse(text.strip()) return 10 except SyntaxError: return 0 def validate_regex(text): try: re.compile(text.strip()) return 10 except re.error: return 0

Each function tries to parse the text as its respective format. If parsing succeeds, it returns a perfect score of 10. If it fails with an error, the syntax is invalid and returns 0.

Dataset Format Requirements
For the code grader to know which validator to use, your test cases need to specify the expected output format:
{ "task": "Create a Python function to validate an AWS IAM username", "format": "python" }

You can update your dataset generation prompt to automatically include this format field by adding it to the example output structure.

Improving Prompt Clarity
To get better results from your AI model, make your prompt instructions more specific about the expected output format:
- Respond only with Python, JSON, or a plain Regex
- Do not add any comments or commentary or explanation

You can also use a pre-filled assistant message with code blocks to encourage the model to return just the raw code:
add_assistant_message(messages, "```code")

This tells Claude to start generating code content without having to specify whether it's Python, JSON, or Regex ahead of time.

Combining Scores
The final step is merging the model grader score with the code grader score. A simple approach is to take the average:

> *(See full lesson at course URL)*

#### 22. Exercise on prompt evals


#### 23. Quiz on prompt evaluation



### 📖 Prompt engineering techniques

#### 24. Prompt engineering

Prompt engineering is about taking a prompt you've written and improving it to get more reliable, higher-quality outputs. This process involves iterative refinement - starting with a basic prompt, evaluating its performance, then systematically applying engineering techniques to improve it.

The Iterative Improvement Process
The approach follows a clear cycle that you can repeat until you achieve your desired results:
- Set a goal - Define what you want your prompt to accomplish
- Write an initial prompt - Create a basic first attempt
- Evaluate the prompt - Test it against your criteria
- Apply prompt engineering techniques - Use specific methods to improve performance
- Re-evaluate - Verify that your changes actually improved the results

You repeat the last two steps until you're satisfied with the performance. Each iteration should show measurable improvement in your evaluation scores.

Setting Up Your Evaluation Pipeline
To demonstrate this process, we'll work with a practical example: creating a prompt that generates one-day meal plans for athletes. The prompt needs to take into account an athlete's height, weight, goals, and dietary restrictions, then produce a comprehensive meal plan.

The evaluation setup uses a PromptEvaluator class that handles dataset generation and model grading. When creating your evaluator instance, you can control concurrency with the max_concurrent_tasks parameter:
evaluator = PromptEvaluator(max_concurrent_tasks=5)

Start with a low concurrency value (like 3) to avoid rate limit errors. You can increase it if your API quota allows for faster processing.

Generating Test Data
The evaluation system can automatically generate test cases based on your prompt requirements. You define what inputs your prompt needs:
dataset = evaluator.generate_dataset( task_description="Write a compact, concise 1 day meal plan for a single athlete", prompt_inputs_spec={ "height": "Athlete's height in cm", "weight": "Athlete's weight in kg", "goal": "Goal of the athlete", "restrictions": "Dietary restrictions of the athlete" }, output_file="dataset.json", num_cases=3 )

Keep the number of test cases low (2-3) during development to speed up your iteration cycle. You can increase this for final validation.

Writing Your Initial Prompt
Start with a simple, naive prompt to establish a baseline. Here's an example of a deliberately basic first attempt:

> *(See full lesson at course URL)*

#### 25. Being clear and direct

The first line of your prompt is the most important part of your entire request. This is where you set the stage for everything that follows, and getting it right can dramatically improve your results.

Being Clear and Direct
When crafting that crucial first line, you want to focus on two key principles: clarity and directness. This means using simple language that leaves no room for ambiguity about what you want Claude to do.

Clear Communication
Being "clear" means:
- Use simple language that anyone can understand
- State exactly what you want without beating around the bush
- Lead with a straightforward statement of Claude's task

Instead of writing something vague like "I need to know about those things people put on their roofs that use sun - those solar panel things, I think they're called," be direct and write: "Write three paragraphs about how solar panels work."

Direct Instructions
Being "direct" focuses on how you structure your request:
- Use instructions, not questions
- Start with direct action verbs like "Write," "Create," or "Generate"

Rather than asking "I was reading about renewable energy and geothermal energy sounds neat. What countries use it?" try: "Identify three countries that use geothermal energy. Include generation stats for each."

Putting It Into Practice
Let's see this technique in action. Starting with a weak prompt that simply asked "What should this person eat?" we can apply our clear and direct approach.

The improved version becomes:
Generate a one-day meal plan for an athlete that meets their dietary restrictions.

This revision immediately tells Claude:
- What action to take (generate)
- What to create (a meal plan)
- Key constraints (one day, for an athlete, meeting dietary restrictions)

Results Matter
This simple change can have a significant impact on performance. In our example, the evaluation score jumped from 2.32 to 3.92 - a substantial improvement from just restructuring that opening line.

The key takeaway is that Claude responds best when you treat it like a capable assistant who needs clear direction rather than someone who has to guess what you want. Start strong with a direct action verb, be specific about the task, and you'll see better results right away.

#### 26. Being specific

When working with Claude, one of the most effective ways to improve your results is to be specific about what you want. Instead of leaving everything up to the model's interpretation, you can provide clear guidelines or steps that direct Claude toward the kind of output you're looking for.

Think about it this way: if you ask Claude to "write a short story about a character who discovers a hidden talent," Claude could go in countless directions. The story might be 200 words or 2,000 words. It might have one character or five. It could focus on any type of talent discovery scenario.

By adding specific guidelines, you give Claude a clearer target to aim for. This dramatically improves both the consistency and quality of the output.

Two Types of Guidelines
There are two main approaches to being specific in your prompts, and you'll often see them used together in professional applications.

Output Quality Guidelines
The first type focuses on listing qualities that your output should have. These guidelines help you control:
- Length of the response
- Structure and format
- Specific attributes or elements to include
- Tone or style requirements

For example, you might specify that a story should be under 1,000 words, include a clear action that reveals the character's talent, and feature at least one supporting character.

Process Steps
The second type provides specific steps for Claude to follow. This approach is particularly useful when you want Claude to think through a problem systematically or consider multiple perspectives before arriving at a final answer.

Instead of jumping straight to writing, you might ask Claude to:
- Brainstorm three talents that would create dramatic tension
- Pick the most interesting talent
- Outline a pivotal scene that reveals the talent
- Brainstorm supporting character types that could increase the impact

Real-World Impact
The difference that specificity makes is dramatic. In testing a meal planning prompt, adding guidelines improved the evaluation score from 3.92 to 7.86 - more than doubling the quality of the output simply by telling Claude exactly what elements to include.

Guidelines: 1. Include accurate daily calorie amount 2. Show protein, fat, and carb amounts 3. Specify when to eat each meal 4. Use only foods that fit restrictions 5. List all portion size in grams 6. Keep budget-friendly if mentioned

When to Use Each Approach
Here's a practical guide for when to use each type of specificity:


> *(See full lesson at course URL)*

#### 27. Structure with XML tags

When you're building prompts that include a lot of content, Claude can sometimes struggle to understand which pieces of text belong together or what different sections are supposed to represent. XML tags provide a simple way to add structure and clarity to your prompts, especially when you're interpolating large amounts of data.

Why Structure Matters
Consider a prompt where you need to analyze 20 pages of sales records. Without clear boundaries, Claude might have trouble distinguishing between your instructions and the actual data you want analyzed.
The example shows how unclear boundaries can make it difficult for Claude to parse your intent. By wrapping the sales records in XML tags like <sales_records> and </sales_records>, you create clear delimiters that help Claude understand the structure of your prompt.

Practical Example: Code and Documentation
Here's a more dramatic example of why XML tags matter. If you ask Claude to debug code using provided documentation, mixing everything together creates confusion.
The Not Great version makes it nearly impossible to tell what's code versus documentation. The Better version uses <my_code> and <docs> tags to create clear boundaries.

Custom Tag Names
You don't need to use official XML tags. Create descriptive names that make sense for your content:
<sales_records> is better than <data>
<athlete_information> clearly identifies user details
<my_code> and <docs> separate different types of content
The more specific and descriptive your tag names, the better Claude can understand the purpose of each section.

When to Use XML Tags
XML tags are most useful when:
- Including large amounts of context or data
- Mixing different types of content (code, documentation, data)
- You want to be extra clear about content boundaries
- Working with complex prompts that interpolate multiple variables
Even for shorter content, XML tags can help serve as delimiters that make your prompt structure more obvious to Claude.

Real-World Application
In practice, you might structure a prompt like this:
<athlete_information>
- Height: 6'2"
- Weight: 180 lbs
- Goal: Build muscle
- Dietary restrictions: Vegetarian
</athlete_information>
Generate a meal plan based on the athlete information above.
This makes it crystal clear that the height, weight, goal, and restrictions are all related athlete data that should be considered together when generating the meal plan.

> *(See full lesson at course URL)*

#### 28. Providing examples

Providing examples in your prompts is one of the most effective prompt engineering techniques you'll use. This approach, known as one-shot or multi-shot prompting, involves giving Claude sample input/output pairs to guide its responses.

How Examples Work
Let's look at a sentiment analysis example. Say you want Claude to categorize whether a tweet is positive or negative.
The challenge here is sarcasm. A tweet like Yeah, sure, that was the best movie I've seen since 'Plan 9 from Outer Space' appears positive on the surface, but it's actually sarcastic and negative (Plan 9 is famously one of the worst movies ever made).

Adding Examples to Handle Corner Cases
To solve this, you can add examples that show Claude how to handle tricky cases:
The improved prompt includes:
- A clear positive example: Great game tonight! → Positive
- A sarcastic example: Oh yeah, I really needed a flight delay tonight! Excellent! → Negative
- Context explaining why sarcasm should be treated carefully
Notice how the examples are wrapped in XML tags like <sample_input> and <ideal_output>. This structure makes it crystal clear to Claude what each part represents.

When to Use Examples
Examples are particularly useful for:
- Capturing corner cases or edge scenarios
- Defining complex output formats (like specific JSON structures)
- Showing the exact style or tone you want
- Demonstrating how to handle ambiguous inputs

One-Shot vs Multi-Shot
One-Shot: Provide a single example to establish the pattern
Multi-Shot: Provide multiple examples to cover different scenarios
Use multi-shot when you need to handle various edge cases or want to show different types of valid responses.

Finding Good Examples from Evaluations
When running prompt evaluations, look for your highest-scoring outputs to use as examples:
Find responses that scored 10 (or your highest available score) and use those input/output pairs as examples in your prompt. This helps Claude understand what perfect output looks like for your specific use case.

Adding Context to Examples
Don't just provide the input/output pair - explain why the output is good:
<ideal_output> [Your example output here] </ideal_output>
This example is well-structured, provides detailed information on food choices and quantities, and aligns with the athlete's goals and restrictions.
This additional context helps Claude understand the reasoning behind good responses, not just the format.

Best Practices

> *(See full lesson at course URL)*

#### 29. Exercise on prompting


#### 30. Quiz on prompt engineering techniques



### 📖 Tool use with Claude

#### 31. Introducing tool use

Tools allow Claude to access information from the outside world, extending its capabilities beyond what it learned during training. By default, Claude only knows information from its training data and can't access current events, real-time data, or external systems. Tool use solves this limitation by creating a structured way for Claude to request and receive fresh information.

The Problem Without Tools
When users ask Claude for current information, it hits a wall. For example, if someone asks "What's the weather in San Francisco, California?" Claude has to respond with something like "I'm sorry, but I don't have access to up-to-date weather information."
This creates a frustrating user experience when people need real-time data that Claude could theoretically help with if it just had access to current information.

How Tool Use Works
Tool use follows a specific back-and-forth pattern between your application and Claude. Here's the complete flow:
- Initial Request: You send Claude a question along with instructions on how to get extra data from external sources
- Tool Request: Claude analyzes the question and decides it needs additional information, then asks for specific details about what data it needs
- Data Retrieval: Your server runs code to fetch the requested information from external APIs or databases
- Final Response: You send the retrieved data back to Claude, which then generates a complete response using both the original question and the fresh data

Weather Example in Practice
Let's see how this works with the weather question. The process becomes much more specific:
When a user asks about current weather, you include instructions in your prompt about how to retrieve weather data. Claude recognizes it needs current information and requests weather data for the specific location. Your server then calls a weather API to get real-time conditions and sends that data back to Claude. Finally, Claude combines the fresh weather data with the user's question to provide an accurate, current response.

Key Benefits
- Real-time Information: Access current data that wasn't available during Claude's training
- External System Integration: Connect Claude to databases, APIs, and other services
- Dynamic Responses: Provide answers based on the latest available information
- Structured Interaction: Claude knows exactly what information it needs and how to ask for it

> *(See full lesson at course URL)*

#### 32. Project overview

We're going to build a practical project that teaches Claude how to set reminders for future dates. This might sound simple at first, but it reveals several interesting challenges that we'll solve using custom tools.

The goal is straightforward: we want to be able to tell Claude "Set a reminder for my doctor's appointment. It's a week from Thursday" and have Claude respond with "OK, I will remind you." But to make this work, we need to address some limitations in how Claude handles time and reminders.

Why This Is Challenging
While Claude knows the current date, there are three specific problems we need to solve:
- Limited time awareness: Claude might know the current date, but not the exact time
- Date calculation issues: Claude isn't perfect with time-based addition, especially when looking many days into the future
- No reminder capability: Claude doesn't know how to set a reminder - it has no built-in mechanism for this

Each of these limitations represents a gap between what Claude can do naturally and what we need for our reminder system. Tools are how we bridge these gaps.

Tools We Need
We'll create three separate tools to handle each challenge:
- Get the current date time: Claude needs to know the current date and time precisely
- Add duration to date time: Claude isn't perfect with date time addition, so we'll give it a reliable tool for this
- Set a reminder: We need a way to actually set a reminder in the system

We'll implement these tools one at a time, starting with the simplest one. This approach lets us understand how tool calling works before building more complex functionality. By the end, Claude will be able to handle natural language requests like "remind me in a week" by combining these tools to calculate the exact time and set the reminder.

This project demonstrates a key principle of working with AI: when the model has limitations, we extend its capabilities through tools rather than trying to work around those limitations in our prompts.

#### 33. Tool functions

When building AI applications with Claude, you'll often need to give it access to real-time information or the ability to perform actions. This is where tool functions come in - they're Python functions that Claude can call when it needs additional data to help users.

What Are Tool Functions?
A tool function is a plain Python function that gets executed automatically when Claude decides it needs extra information to help a user. For example, if someone asks "What time is it?", Claude would call your date/time tool to get the current time.

Here's an example of a weather tool function. Notice how it validates inputs and provides clear error messages - these are important best practices.

Best Practices for Tool Functions
When writing tool functions, follow these guidelines:
- Use descriptive names: Both your function name and parameter names should clearly indicate their purpose
- Validate inputs: Check that required parameters aren't empty or invalid, and raise errors when they are
- Provide meaningful error messages: Claude can see error messages and might retry the function call with corrected parameters

The validation is particularly important because Claude learns from errors. If you raise a clear error like "Location cannot be empty", Claude might try calling the function again with a proper location value.

Building Your First Tool Function
Let's create a function to get the current date and time. This function will accept a date format parameter so Claude can request the time in different formats:
def get_current_datetime(date_format="%Y-%m-%d %H:%M:%S"): if not date_format: raise ValueError("date_format cannot be empty") return datetime.now().strftime(date_format)

This function uses Python's datetime module to get the current time and format it according to the provided format string. The default format gives us year-month-day hour:minute:second.

You can test it with different formats:
# Default format: "2024-01-15 14:30:25" get_current_datetime() # Just hour and minute: "14:30" get_current_datetime("%H:%M")

The validation check ensures Claude can't pass an empty string for the date format. While this specific error is unlikely, it demonstrates the pattern of validating inputs and providing helpful error messages that Claude can learn from.

Next Steps

> *(See full lesson at course URL)*

#### 34. Tool schemas

After writing your tool function, the next step is creating a JSON schema that tells Claude what arguments your function expects and how to use it. This schema acts as documentation that Claude reads to understand when and how to call your tools.

Understanding JSON Schema
JSON Schema isn't specific to AI or tool calling - it's a widely-used data validation specification that's been around for years. The AI community adopted it because it's a convenient way to describe function parameters and validate data.

The complete tool specification has three main parts:
- name: A clear, descriptive name for your tool (like "get_weather")
- description: What the tool does, when to use it, and what it returns
- input_schema: The actual JSON schema describing the function's arguments

Writing Effective Descriptions
Your tool description is crucial for helping Claude understand when to use your function. Best practices include:
- Aim for 3-4 sentences explaining what the tool does
- Describe when Claude should use it
- Explain what kind of data it returns
- Provide detailed descriptions for each argument

The Easy Way to Generate Schemas
Instead of writing JSON schemas from scratch, you can use Claude itself to generate them. Here's the process:
- Copy your tool function code
- Go to Claude and ask it to write a JSON schema for tool calling
- Include the Anthropic documentation on tool use as context
- Let Claude generate a properly formatted schema following best practices

The prompt should be something like: "Write a valid JSON schema spec for the purposes of tool calling for this function. Follow the best practices listed in the attached documentation."

Implementing the Schema in Code
Once Claude generates your schema, copy it into your code file. Here's a good naming pattern to follow:
def get_current_datetime(date_format="%Y-%m-%d %H:%M:%S"): if not date_format: raise ValueError("date_format cannot be empty") return datetime.now().strftime(date_format) get_current_datetime_schema = { "name": "get_current_datetime", "description": "Returns the current date and time formatted according to the specified format", "input_schema": { "type": "object", "properties": { "date_format": { "type": "string", "description": "A string specifying the format of the returned datetime. Uses Python's strftime format codes.", "default": "%Y-%m-%d %H:%M:%S" } }, "required": [] } }


> *(See full lesson at course URL)*

#### 35. Handling message blocks

When working with Claude's tool functionality, you'll encounter a new type of response structure that's different from the simple text responses you've seen before. Instead of just getting back a single text block, Claude can now return multi-block messages that contain both text and tool usage information.

Making Tool-Enabled API Calls
To enable Claude to use tools, you need to include a tools parameter in your API call. Here's how to structure the request:
messages = [] messages.append({ "role": "user", "content": "What is the exact time, formatted as HH:MM:SS?" }) response = client.messages.create( model=model, max_tokens=1000, messages=messages, tools=[get_current_datetime_schema], )

The tools parameter takes a list of JSON schemas that describe the available functions Claude can call.

Understanding Multi-Block Messages
When Claude decides to use a tool, it returns an assistant message with multiple blocks in the content list. This is a significant change from the simple text-only responses you've worked with before.

A multi-block message typically contains:
- Text Block: Human-readable text explaining what Claude is doing (like "I can help you find out the current time. Let me find that information for you")
- ToolUse Block: Instructions for your code about which tool to call and what parameters to use

The ToolUse block includes:
- An ID for tracking the tool call
- The name of the function to call (like "get_current_datetime")
- Input parameters formatted as a dictionary
- The type designation "tool_use"

Managing Conversation History with Multi-Block Messages
Remember that Claude doesn't store conversation history - you need to manage it manually. When working with tool responses, you must preserve the entire content structure, including all blocks.

Here's how to properly append a multi-block assistant message to your conversation history:
messages.append({ "role": "assistant", "content": response.content })

This preserves both the text block and the tool use block, which is crucial for maintaining the conversation context when you make subsequent API calls.

The Complete Tool Usage Flow
The tool usage process follows this pattern:
- Send user message with tool schema to Claude
- Receive assistant message with text block and tool use block
- Extract tool information and execute the actual function
- Send tool result back to Claude along with complete conversation history
- Receive final response from Claude


> *(See full lesson at course URL)*

#### 36. Sending tool results

After Claude requests a tool call, you need to execute the function and send the results back. This completes the tool use workflow by providing Claude with the information it requested.

Running the Tool Function
When Claude responds with a tool use block, you extract the input parameters and call your function. Here's how to access the tool parameters:
response.content[1].input

This gives you a dictionary of the arguments Claude wants to pass to your function. Since your function expects keyword arguments rather than a dictionary, you use Python's unpacking syntax:
get_current_datetime(**response.content[1].input)

Tool Result Block
After running the tool function, you need to send the results back to Claude using a tool result block. This block goes inside a user message and tells Claude what happened when you executed the tool.

The tool result block has several important properties:
- tool_use_id: Must match the id of the ToolUse block that this ToolResult corresponds to
- content: Output from running your tool, serialized as a string
- is_error: True if an error occurred

Handling Multiple Tool Calls
Claude can request multiple tool calls in a single response. For example, if a user asks "What's 10 + 10 and what's 30 + 30?", Claude might respond with two separate ToolUse blocks.

Each tool call gets a unique ID, and you must match these IDs when sending back results. This ensures Claude knows which result corresponds to which request, even if the results arrive in a different order.

Building the Follow-up Request
Your follow-up request to Claude must include the complete conversation history plus the new tool result. Here's the structure:
messages.append({ "role": "user", "content": [{ "type": "tool_result", "tool_use_id": response.content[1].id, "content": "15:04:22", "is_error": False }] })

The complete message history now contains:
- Original user message
- Assistant message with tool use block
- User message with tool result block

Making the Final Request
When sending the follow-up request, you must still include the tool schema even though you're not expecting Claude to make another tool call. Claude needs the schema to understand the tool references in your conversation history.
client.messages.create( model=model, max_tokens=1000, messages=messages, tools=[get_current_datetime_schema] )


> *(See full lesson at course URL)*

#### 37. Multi-turn conversations with tools

When building applications with multiple tools, you need to handle scenarios where Claude might need to call several tools in sequence to answer a single user question. For example, if a user asks "What day is 103 days from today?", Claude needs to first get the current date, then add 103 days to it.

This creates a multi-turn conversation pattern where Claude makes multiple tool requests before providing a final answer. Your application needs to handle this automatically.

The Multi-Turn Tool Pattern
Here's what happens behind the scenes when Claude needs multiple tools:
1. User asks: "What day is 103 days from today?" - Claude responds with a tool use block requesting get_current_datetime
2. Your server calls the function and returns the result - Claude realizes it needs more information and requests add_duration_to_datetime
3. Your server calls that function and returns the result
4. Claude now has enough information to provide the final answer

Building a Conversation Loop
To handle this pattern, you need a conversation loop that continues until Claude stops requesting tools:
def run_conversation(messages): while True: response = chat(messages) add_user_message(messages, response) # Pseudo code if response isn't asking for a tool: break tool_result_blocks = run_tools(response) add_user_message(tool_result_blocks) return messages

Refactoring Helper Functions
Before implementing the conversation loop, you need to update your helper functions to handle multiple message blocks properly.

Updating Message Handlers
Your add_user_message and add_assistant_message functions currently assume you're always working with plain text. Update them to handle full message objects:
from anthropic.types import Message def add_user_message(messages, message): user_message = { "role": "user", "content": message.content if isinstance(message, Message) else message } messages.append(user_message)

This allows you to pass in either a string, a list of blocks, or a complete message object.

Updating the Chat Function
Modify your chat function to accept a list of tools and return the full message instead of just text:
def chat(messages, system=None, temperature=1.0, stop_sequences=[], tools=None): params = { "model": model, "max_tokens": 1000, "messages": messages, "temperature": temperature, "stop_sequences": stop_sequences, } if tools: params["tools"] = tools if system: params["system"] = system message = client.messages.create(**params) return message


> *(See full lesson at course URL)*

#### 38. Implementing multiple turns

Building a conversation system with tools requires implementing a loop that keeps calling Claude until it stops requesting tool usage. When Claude no longer asks for tools, that signals it has a final response ready for the user.

Detecting Tool Requests
The key to knowing whether Claude wants to use a tool lies in the stop_reason field of the response message. When Claude decides it needs to call a tool, this field gets set to "tool_use". This gives us a clean way to check if we need to continue the conversation loop:
if response.stop_reason != "tool_use": break # Claude is done, no more tools needed

The Conversation Loop
The main conversation function follows a simple pattern:
def run_conversation(messages): while True: response = chat(messages, tools=[get_current_datetime_schema]) add_assistant_message(messages, response) print(text_from_message(response)) if response.stop_reason != "tool_use": break tool_results = run_tools(response) add_user_message(messages, tool_results) return messages

This loop continues until Claude provides a final answer without requesting any tools.

Handling Multiple Tool Calls
Claude can request multiple tools in a single response. The message content contains a list of blocks, and we need to process each tool use block separately:
The run_tools function handles this by filtering for tool use blocks and processing each one:
def run_tools(message): tool_requests = [ block for block in message.content if block.type == "tool_use" ] tool_result_blocks = [] for tool_request in tool_requests: # Process each tool request...

Tool Result Blocks
Each tool use block must be answered with a corresponding tool result block. The connection between them is maintained through matching IDs:
The tool result block structure includes:
tool_result_block = { "type": "tool_result", "tool_use_id": tool_request.id, "content": json.dumps(tool_output), "is_error": False }

Error Handling
Robust tool execution requires handling potential errors. When a tool fails, we still need to provide a result block to Claude:
try: tool_output = run_tool(tool_request.name, tool_request.input) tool_result_block = { "type": "tool_result", "tool_use_id": tool_request.id, "content": json.dumps(tool_output), "is_error": False } except Exception as e: tool_result_block = { "type": "tool_result", "tool_use_id": tool_request.id, "content": f"Error: {e}", "is_error": True }

Scalable Tool Routing

> *(See full lesson at course URL)*

#### 39. Using multiple tools

Adding multiple tools to your Claude implementation becomes straightforward once you have the core tool-handling infrastructure in place. This tutorial shows how to integrate additional tools by following a simple pattern.

The Tools We're Adding
We need three main capabilities for our reminder system:
- Get current date time - Claude needs to know the current date and time
- Add duration to date time - Claude isn't perfect with date time addition
- Set a reminder - Need a way to set a reminder
The good news is that most of the implementation work is already done. The add_duration_to_datetime function and set_reminder function are provided, along with their corresponding schemas.

Adding Tools to the Conversation
First, update the run_conversation function to include the new tool schemas in the tools list:
response = chat(messages, tools=[ get_current_datetime_schema, add_duration_to_datetime_schema, set_reminder_schema ])

This tells Claude about all three available tools it can use during the conversation.

Updating the Tool Router
Next, modify the run_tool function to handle the new tool calls. Add elif cases for each new tool:
def run_tool(tool_name, tool_input): if tool_name == "get_current_datetime": return get_current_datetime(**tool_input) elif tool_name == "add_duration_to_datetime": return add_duration_to_datetime(**tool_input) elif tool_name == "set_reminder": return set_reminder(**tool_input)

The pattern is simple: check the tool name, call the corresponding function with the provided input, and return the result.

Testing Multiple Tool Usage
To test the system, try a request that requires multiple tools: "Set a reminder for my doctors appointment. Its 177 days after Jan 1st, 2050."

This request forces Claude to:
- Calculate the date (using add_duration_to_datetime)
- Set the reminder (using set_reminder)

Claude handles this by first explaining what it needs to do, then making the appropriate tool calls in sequence. The conversation shows Claude calculating June 27, 2050 as the target date, then setting the reminder for that date.

Understanding the Message Flow
When you examine the conversation history, you'll see the complete message structure:
1. User message with the request
2. Assistant message containing both text and tool use blocks
3. Tool result messages
4. Follow-up assistant messages

This demonstrates how Claude can include multiple blocks in a single message - combining explanatory text with tool usage requests.


> *(See full lesson at course URL)*

#### 40. Fine grained tool calling

When you combine tool use with streaming in Claude, you get real-time updates as the AI generates tool arguments. This creates a more responsive user experience, but there are some important details to understand about how it works behind the scenes.

Basic Tool Streaming
With streaming enabled, Claude sends back different types of events as it processes your request. You're already familiar with events like ContentBlockDelta for regular text generation. For tool use, you'll also need to handle a new event type called InputJsonEvent.

Each InputJsonEvent contains two key properties:
- partial_json - A chunk of JSON representing part of the tool arguments
- snapshot - The cumulative JSON built up from all chunks received so far

Here's how you handle these events in your streaming pipeline:
for chunk in stream: if chunk.type == "input_json": # Process the partial JSON chunk print(chunk.partial_json) # Or use the complete snapshot so far current_args = chunk.snapshot

How JSON Validation Works
Here's where things get interesting. The Anthropic API doesn't immediately send you every chunk as Claude generates it. Instead, it buffers chunks and validates them first.

The API waits for complete top-level key-value pairs before sending anything. For example, if your tool expects this structure:
{ "abstract": "This paper presents a novel...", "meta": { "word_count": 847, "review": "This paper introduces QuanNet..." } }

The API will:
1. Wait until the entire abstract value is complete
2. Validate that key-value pair against your schema
3. Send all the buffered chunks for abstract at once
4. Repeat the process for the meta object

This validation process explains why you see delays followed by bursts of text, even with streaming enabled. The chunks are being held back until a complete, valid top-level key-value pair is ready.

Fine-Grained Tool Calling
If you need faster, more granular streaming - perhaps to show users immediate updates or start processing partial results quickly - you can enable fine-grained tool calling.

Fine-grained tool calling does one main thing: it disables JSON validation on the API side. This means:
- You get chunks as soon as Claude generates them
- No buffering delays between top-level keys
- More traditional streaming behavior
- Critical: JSON validation is disabled - your code must handle invalid JSON

Enable it by adding fine_grained=True to your API call:
run_conversation( messages, tools=[save_article_schema], fine_grained=True )


> *(See full lesson at course URL)*

#### 41. The text edit tool

Claude comes with one built-in tool that you don't need to create from scratch: the text editor tool. This tool gives Claude the ability to work with files and directories just like you would in a standard text editor.

What the Text Editor Tool Can Do
The text editor tool provides Claude with a comprehensive set of file manipulation capabilities:
- View file or directory contents
- View specific ranges of lines in a file
- Replace text in a file
- Create new files
- Insert text at specific lines in a file
- Undo recent edits to files

This dramatically expands Claude's abilities and essentially gives it the power to act as a software engineer right out of the gate.

Understanding the Implementation Requirements
Here's where things get a bit confusing: while the tool schema is built into Claude, you still need to provide the actual implementation. Think of it this way - Claude knows how to ask for file operations, but you need to write the code that actually performs those operations.

When you use other tools, you write both the JSON schema and the function implementation. With the text editor tool, Claude provides the schema knowledge, but you must write functions to handle Claude's requests to create files, read directories, replace text, and so on.

Schema Versions
While the main schema is built into Claude, you do need to include a small schema stub when making requests. The exact schema depends on which Claude model you're using:
def get_text_edit_schema(model): if model.startswith("claude-3-7-sonnet"): return { "type": "text_editor_20250124", "name": "str_replace_editor", } elif model.startswith("claude-3-5-sonnet"): return { "type": "text_editor_20241022", "name": "str_replace_editor", }

Claude sees this small schema and automatically expands it into the full text editor tool specification behind the scenes.

Practical Example
Let's see the text editor tool in action. When you ask Claude to work with files, it will use the tool to read, modify, and create files as needed.

For example, if you ask Claude to "Open the ./main.py file and summarize its contents", Claude will:
1. Use the text editor tool to view the file
2. Read the contents
3. Provide you with a summary

You can take this further by asking Claude to modify files. For instance: "Open the ./main.py file and write out a function to calculate pi to the 5th digit. Then create a ./test.py file to test your implementation."

Claude will:
1. View the existing main.py file

> *(See full lesson at course URL)*

#### 42. The web search tool

Claude includes a built-in web search tool that lets it search the internet for current or specialized information to answer user questions. Unlike other tools where you need to provide the implementation, Claude handles the entire search process automatically - you just need to provide a simple schema to enable it.

Setting Up the Web Search Tool
To use the web search tool, you create a schema object with these required fields:
web_search_schema = { "type": "web_search_20250305", "name": "web_search", "max_uses": 5 }

The max_uses field limits how many searches Claude can perform. Claude might do follow-up searches based on initial results, so this prevents excessive API calls. A single search returns multiple results, but Claude may decide additional searches are needed.

How the Response Works
When Claude uses the web search tool, the response contains several types of blocks:
- Text blocks - Claude's explanation of what it's doing
- ServerToolUseBlock - Shows the exact search query Claude used
- WebSearchToolResultBlock - Contains the search results
- WebSearchResultBlock - Individual search results with titles and URLs
- Citation blocks - Text that supports Claude's statements

The response structure lets you see exactly what Claude searched for and which sources it found. Citations include the specific text Claude used to support its answers, along with the source URLs.

Restricting Search Domains
You can limit searches to specific domains using the allowed_domains field. This is particularly useful when you want reliable, authoritative sources:
web_search_schema = { "type": "web_search_20250305", "name": "web_search", "max_uses": 5, "allowed_domains": ["nih.gov"] }

For example, when asking about medical or exercise advice, restricting to domains like PubMed (nih.gov) ensures you get evidence-based information rather than random blog content.

Rendering Search Results
The different block types in the response are designed for specific UI rendering:
- Render text blocks as regular content
- Display web search results as a list of sources at the top
- Show citations inline with the text, including the source domain, page title, URL, and quoted text

This structure helps users understand how Claude arrived at its answers and provides transparency about the sources being used. The citation format makes it clear which specific information came from which sources, building trust in the AI's responses.

Practical Usage
The web search tool works best for:

> *(See full lesson at course URL)*

#### 43. Quiz on tool use with Claude



### 📖 RAG and Agentic Search

#### 44. Introducing Retrieval Augmented Generation

Retrieval Augmented Generation (RAG) is a technique that helps you work with large documents that are too big to fit into a single prompt. Instead of cramming everything into one massive prompt, RAG breaks documents into chunks and only includes the most relevant pieces when answering questions.

The Problem with Large Documents
Imagine you have an 800-page financial document and want to ask Claude specific questions about it, like "What risk factors does this company have?" You need to get the relevant information from the document to Claude somehow, but there are limits to how much text you can include in a prompt.

Option 1: Include Everything in the Prompt
The first approach is straightforward - extract all text from the document and stuff it into your prompt along with the user's question. Your prompt might look like this:
Answer the user's question about the financial document. <user_question> {user_question} </user_question> <financial_document> {financial_document} </financial_document>

This approach has serious limitations:
- There's a hard limit on prompt length - your document might be too long
- Claude becomes less effective with very long prompts
- Larger prompts cost more to process
- Larger prompts take longer to process

Option 2: Break Documents into Chunks
RAG takes a smarter approach. First, you break the document into smaller chunks during a preprocessing step. Then, when a user asks a question, you find the chunks most relevant to their question and only include those in your prompt.

Here's how it works: if someone asks "What risks does this company face?" you'd search through your chunks, find the "Risk Factors" section, and include just that relevant chunk in your prompt.

Benefits of RAG
- Claude can focus on only the most relevant content
- Scales up to very large documents
- Works with multiple documents
- Smaller prompts cost less and run faster

Challenges with RAG
- Requires a preprocessing step to chunk documents
- Need a search mechanism to find "relevant" chunks
- Included chunks might not contain all the context Claude needs
- Many ways to chunk text - which approach is best?

For example, you could split documents into equal-sized portions, or you could create chunks based on document structure like headers and sections. Each approach has trade-offs you'll need to evaluate for your specific use case.

When to Use RAG

> *(See full lesson at course URL)*

#### 45. Text chunking strategies

Text chunking is one of the most critical steps in building a RAG (Retrieval Augmented Generation) pipeline. How you break up your documents directly impacts the quality of your entire system. A poor chunking strategy can lead to irrelevant context being inserted into your prompts, causing your AI to give completely wrong answers.

Consider this example: you have a document with sections on medical research and software engineering. If you chunk poorly, a user asking "How many bugs did engineers fix this year?" might get information about medical research instead of software engineering, simply because the medical section happened to contain the word "bug" in a different context.

This is why choosing the right chunking strategy matters so much. Let's explore three main approaches.

Size-Based Chunking
Size-based chunking is the simplest approach - you divide your text into strings of equal length. If you have a 325-character document, you might split it into three chunks of roughly 108 characters each.

This method is easy to implement and works with any type of document, but it has clear downsides:
- Words get cut off mid-sentence
- Chunks lose important context from surrounding text
- Section headers might be separated from their content

To address these issues, you can add overlap between chunks. This means each chunk includes some characters from the neighboring chunks, providing better context and ensuring complete words and sentences.

Here's a basic implementation:
def chunk_by_char(text, chunk_size=150, chunk_overlap=20): chunks = [] start_idx = 0 while start_idx < len(text): end_idx = min(start_idx + chunk_size, len(text)) chunk_text = text[start_idx:end_idx] chunks.append(chunk_text) start_idx = ( end_idx - chunk_overlap if end_idx < len(text) else len(text) ) return chunks

Structure-Based Chunking
Structure-based chunking divides text based on the document's natural structure - headers, paragraphs, and sections. This works great when you have well-formatted documents like Markdown files.

For a Markdown document, you can split on header markers:
def chunk_by_section(document_text): pattern = r"\n## " return re.split(pattern, document_text)

This approach gives you the cleanest, most meaningful chunks because each one represents a complete section. However, it only works when you have guarantees about your document structure. Many real-world documents are plain text or PDFs without clear structural markers.

Semantic-Based Chunking

> *(See full lesson at course URL)*

#### 46. Text embeddings


#### 47. The full RAG flow

Now that we've covered the basics of RAG, text chunking, and embeddings, let's walk through the complete RAG pipeline step by step. This example will show you exactly how all these pieces work together to retrieve relevant information and generate responses.

Step 1: Chunk Your Source Text

First, we take our source document and break it into manageable chunks. For this example, we'll use two simple text sections:

Section 1: Medical Research - "This year saw significant strides in our understanding of XDR-47, a 'bug' we have not seen before."
Section 2: Software Engineering - "This division dedicated significant effort to studying various infection vectors in our distributed systems"

Step 2: Generate Embeddings

Next, we convert each text chunk into numerical embeddings using an embedding model. To make this easier to understand, let's imagine we have a perfect embedding model that always returns exactly two numbers, and we know what each number represents.

In our imaginary model:
The first number represents how much the text talks about the medical field
The second number represents how much the text talks about software engineering

For the medical research section, we might get [0.97, 0.34] - very medical-focused but with some software elements due to the word "bug". For the software engineering section, we get [0.30, 0.97] - heavily software-focused but with medical undertones from "infection vectors".

Normalization

The embedding API typically performs a normalization step that scales each vector to have a magnitude of 1.0. You don't need to worry about the math here - it's handled automatically. This gives us normalized vectors like [0.944, 0.331] and [0.295, 0.955].

We can visualize these embeddings on a unit circle, where each point represents one of our text chunks.

Step 3: Store in Vector Database

We store these embeddings in a vector database - a specialized database optimized for storing, comparing, and searching through long lists of numbers like our embeddings.

At this point, we pause. All the work so far has been preprocessing that happens ahead of time. Now we wait for a user to submit a query.

Step 4: Process User Query

When a user asks a question like "I'm curious about the company. In particular, what did the software engineering dept do this year?", we run their query through the same embedding model.


> *(See full lesson at course URL)*

#### 48. Implementing the RAG flow


#### 49. BM25 lexical search

When building RAG pipelines, you'll quickly discover that semantic search alone doesn't always return the best results. Sometimes you need exact term matches that semantic search might miss. The solution is to combine semantic search with lexical search using a technique called BM25.

The Problem with Semantic Search Alone

Let's say you're searching for a specific incident ID like "INC-2023-Q4-011" in a document. While semantic search excels at understanding context and meaning, it might return sections that are semantically related but don't actually contain the exact term you're looking for.

In the example above, semantic search returned the cybersecurity section (which does contain the incident ID) but also returned a financial analysis section that doesn't mention the incident at all. This happens because semantic search focuses on conceptual similarity rather than exact term matching.

Hybrid Search Strategy

The solution is to run both semantic and lexical searches in parallel, then merge the results. This gives you the best of both worlds:

Semantic search finds conceptually related content using embeddings
Lexical search finds exact term matches using classic text search
Merged results combine both approaches for better accuracy

How BM25 Works

BM25 (Best Match 25) is a popular algorithm for lexical search in RAG systems. Here's how it processes a search query:

Step 1: Tokenize the query - Break the user's question into individual terms. For example, "a INC-2023-Q4-011" becomes ["a", "INC-2023-Q4-011"].

Step 2: Count term frequency - See how often each term appears across all your documents. Common words like "a" might appear 5 times, while specific terms like "INC-2023-Q4-011" might appear only once.

Step 3: Weight terms by importance - Terms that appear less frequently get higher importance scores. The word "a" gets low importance because it's common, while "INC-2023-Q4-011" gets high importance because it's rare.

Step 4: Find best matches - Return documents that contain more instances of the higher-weighted terms.

Implementing BM25 Search

Here's how to set up a basic BM25 search system:

# 1. Chunk your text by sections
chunks = chunk_by_section(text)

# 2. Create a BM25 store and add documents
store = BM25Index()
for chunk in chunks:
    store.add_document({"content": chunk})

# 3. Search the store
results = store.search("What happened with INC-2023-Q4-011?", 3)

# Print results
for doc, distance in results:

> *(See full lesson at course URL)*

#### 50. A Multi-Index RAG pipeline

We've built separate implementations for semantic search (using vector embeddings) and lexical search (using BM25). Now it's time to combine them into a unified search pipeline that leverages the strengths of both approaches.

The Multi-Index Architecture

Both our VectorIndex and BM25Index classes share nearly identical APIs - they both have add_document() and search() methods. This consistency makes it straightforward to wrap them together in a new class called Retriever.

The Retriever acts as a coordinator that forwards user queries to both indexes, collects their results, and merges them using a technique called reciprocal rank fusion.

Understanding Reciprocal Rank Fusion

Merging results from different search methods isn't as simple as just concatenating lists. Each method uses different scoring systems, so we need a way to normalize and combine their rankings fairly.

Here's how reciprocal rank fusion works with an example. Let's say we search for information about "INC-2023-Q4-011" and get these results:

VectorIndex returns: Section 2 (rank 1), Section 7 (rank 2), Section 6 (rank 3)
BM25Index returns: Section 6 (rank 1), Section 2 (rank 2), Section 7 (rank 3)

We combine these into a single table showing each text chunk's rank from both indexes, then apply the RRF formula:
RRF_score(d) = Σ(1 / (k + rank_i(d)))

Where k is a constant (often 60, but we'll use 1 for clearer results) and rank_i(d) is the rank of document d in the i-th ranking.

For our example:

Section 2: 1.0/(1+1) + 1.0/(1+2) = 0.833
Section 7: 1.0/(1+2) + 1.0/(1+3) = 0.583
Section 6: 1.0/(1+3) + 1.0/(1+1) = 0.75

The final ranking becomes: Section 2 (0.833), Section 6 (0.75), Section 7 (0.583). This makes intuitive sense - Section 2 performed well in both indexes, so it rises to the top.

Implementation Details

The Retriever class wraps multiple search indexes and provides a unified interface:

class Retriever:
    def __init__(self, *indexes: SearchIndex):
        if len(indexes) == 0:
            raise ValueError("At least one index must be provided")
        self._indexes = list(indexes)

    def add_document(self, document: Dict[str, Any]):
        for index in self._indexes:
            index.add_document(document)

    def search(self, query_text: str, k: int = 1, k_rrf: int = 60):
        # Get results from all indexes
        all_results = []
        for idx, results in enumerate(all_results):
            for rank, (doc, _) in enumerate(results):

> *(See full lesson at course URL)*


### 📖 Features of Claude

#### 51. Extended thinking

Important Note: Extended Thinking is not compatible with some other features, notable message pre-filling and temperature. See the full list of restrictions here: https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking#feature-compatibility

Extended thinking is Claude's advanced reasoning feature that gives the model time to work through complex problems before generating a final response. Think of it as Claude's "scratch paper" - you can see the reasoning process that leads to the answer, which helps with transparency and often results in better quality responses.

How Extended Thinking Works

When extended thinking is enabled, Claude's response changes from a simple text block to a structured response containing two parts:

With thinking enabled, you get both the reasoning process and the final answer:

The key benefits include:

Better reasoning capabilities for complex tasks
Increased accuracy on difficult problems
Transparency into Claude's thought process

However, there are important trade-offs:

Higher costs (you pay for thinking tokens)
Increased latency (thinking takes time)
More complex response handling in your code

When to Use Extended Thinking

The decision is straightforward: use your prompt evaluations. Run your prompts without thinking first, and if the accuracy isn't meeting your requirements after you've already optimized your prompt, then consider enabling extended thinking. It's a tool for when standard prompting isn't quite getting you there.

Response Structure and Security

Extended thinking responses include a special signature system for security:

The signature is a cryptographic token that ensures you haven't modified the thinking text. This prevents developers from tampering with Claude's reasoning process, which could potentially lead the model in unsafe directions.

Redacted Thinking

Sometimes you'll receive a redacted thinking block instead of readable reasoning text:

This happens when Claude's thinking process gets flagged by internal safety systems. The redacted content contains the actual thinking in encrypted form, allowing you to pass the complete message back to Claude in future conversations without losing context.

Implementation

To enable extended thinking in your code, you need to add two parameters to your chat function:

def chat( messages, system=None, temperature=1.0, stop_sequences=[], tools=None, thinking=False, thinking_budget=1024 ):


> *(See full lesson at course URL)*

#### 52. Image support

Claude's vision capabilities let you include images in your messages and ask Claude to analyze them in countless ways. You can ask Claude to describe what's in an image, compare multiple images, count objects, or perform complex visual analysis tasks.

Image Handling Basics

There are several important limitations to keep in mind when working with images:
Up to 100 images across all messages in a single request
Max size of 5MB per image
When sending one image: max height/width of 8000px
When sending multiple images: max height/width of 2000px
Images can be included as base64 encoding or a URL to the image
Each image counts as tokens based on its dimensions: tokens = (width px × height px) / 750

To send an image to Claude, you include an image block in your user message alongside text blocks. Here's the structure:
with open("image.png", "rb") as f: image_bytes = base64.standard_b64encode(f.read()).decode("utf-8") add_user_message(messages, [ # Image Block { "type": "image", "source": { "type": "base64", "media_type": "image/png", "data": image_bytes, } }, # Text Block { "type": "text", "text": "What do you see in this image?" } ])

Message Flow

The conversation works just like text-only interactions. Your server sends a user message containing both image and text blocks to Claude, and Claude responds with a text block containing its analysis.

Prompting Techniques

The key to getting good results with images is applying the same prompting engineering techniques you'd use with text. Simple prompts often lead to poor results. For example, asking "How many marbles are in this image?" might return an incorrect count.

You can dramatically improve Claude's accuracy by:
Providing detailed guidelines and analysis steps
Using one-shot or multi-shot examples
Breaking down complex tasks into smaller steps

Step-by-Step Analysis

Instead of a simple question, provide Claude with a methodology:
Analyze this image of marbles and determine the exact count using this methodology: 1. Begin by identifying each unique marble one at a time. Assign each a number as you identify it. 2. Verify your result by counting with a different method. Start from the bottom-left corner and work row by row, from left to right. What is the exact, verified number of marbles in this image?

One-Shot Examples


> *(See full lesson at course URL)*

#### 53. PDF support

Claude can read and analyze PDF files directly, making it a powerful tool for document processing. This capability works similarly to image processing, but with a few key differences in how you structure your code.

Setting Up PDF Processing

To process a PDF file with Claude, you'll use nearly identical code to what you'd use for images. The main differences are in the file type specifications and variable names for clarity.

Here's how to modify your existing image processing code for PDFs:
with open("earth.pdf", "rb") as f: file_bytes = base64.standard_b64encode(f.read()).decode("utf-8") messages = [] add_user_message( messages, [ { "type": "document", "source": { "type": "base64", "media_type": "application/pdf", "data": file_bytes, }, }, {"type": "text", "text": "Summarize the document in one sentence"}, ], ) chat(messages)

Key Changes from Image Processing

When adapting your image processing code for PDFs, you need to update several elements:
Change the file extension from .png to .pdf
Update the variable name from image_bytes to file_bytes for clarity
Set the type to "document" instead of "image"
Change the media type to "application/pdf" instead of "image/png"

What Claude Can Extract from PDFs

Claude's PDF processing capabilities go beyond simple text extraction. It can analyze and understand:
Text content throughout the document
Images and charts embedded in the PDF
Tables and their data relationships
Document structure and formatting

This makes Claude essentially a one-stop solution for extracting any type of information from PDF documents, whether you need summaries, data analysis, or specific content extraction.

The example above shows Claude successfully processing a Wikipedia article about Earth that was saved as a PDF, demonstrating how it can understand and summarize complex document content in a single sentence.

#### 54. Citations

When Claude answers questions based on documents you provide, users might assume it's just drawing from its training data. But what if Claude could show exactly where it found specific information? That's where citations come in - a powerful feature that lets Claude reference specific parts of your source documents and show users exactly where each piece of information comes from.

Why Citations Matter

Imagine asking Claude about how Earth's atmosphere formed and getting a detailed answer. Without citations, users have no way to verify the information or understand that Claude is actually referencing a specific document you provided. Citations solve this transparency problem by creating a clear trail from Claude's response back to your source material.

Enabling Citations

To enable citations, you need to modify your document message structure. Add two new fields to your document block:
{ "type": "document", "source": { "type": "base64", "media_type": "application/pdf", "data": file_bytes, }, "title": "earth.pdf", "citations": { "enabled": True } }

The title field gives your document a readable name, while citations: {"enabled": True} tells Claude to track where it finds information.

Understanding Citation Structure

When citations are enabled, Claude's response becomes more complex. Instead of simple text, you get structured data that includes citation information for each claim.

Each citation contains several key pieces of information:
cited_text - The exact text from your document that supports Claude's statement
document_index - Which document Claude is referencing (useful when you provide multiple documents)
document_title - The title you assigned to the document
start_page_number - Where the cited text begins
end_page_number - Where the cited text ends

Building User Interfaces with Citations

The real power of citations comes from building user interfaces that make this information accessible. You can create interactive elements where users can hover over citation markers to see exactly where information came from.

This creates a transparent experience where users can:
See that Claude's answers are grounded in actual source material
Verify the information by checking the original document
Understand the context around each cited piece of information

Citations with Plain Text

Citations aren't limited to PDF documents. You can also use them with plain text sources. When working with text, modify your document structure like this:

> *(See full lesson at course URL)*

#### 55. Prompt caching

Claude's prompt caching feature is a powerful optimization technique that dramatically reduces costs and improves response times for applications with long system prompts. Rather than sending the full context with every API call, prompt caching lets you preload context that gets referenced efficiently.

How Prompt Caching Works

When you enable prompt caching, Claude stores your system prompt and other fixed context in its key-value cache. On subsequent requests, Claude can quickly reference this cached content without reprocessing it. This means:

You only pay for context once - the first time it's cached
Response times improve significantly since cached content is retrieved faster
Claude maintains full awareness of the cached context

The cache has a maximum size of 200K tokens, so you can include substantial system prompts, instructions, and reference materials. When you make a request with caching enabled, Claude processes both the cached content and any new user input you provide.

Implementation

To enable prompt caching, you add a cache_control field to any message content that you want cached:

messages = [
    {
        "role": "user",
        "content": [
            {
                "type": "text",
                "text": "Here are the company policies:"
            },
            {
                "type": "text",
                "text": employee_handbook,
                "cache_control": {"type": "ephemeral"}
            }
        ]
    }
]

The cache_control field with type "ephemeral" tells Claude to cache this content. The cache is stored in RAM rather than persistent storage, and has a maximum lifetime of 5 minutes once created.

Cache Behavior

The cache operates on a least-recently-used (LRU) basis. When the cache fills up, Claude removes the least recently accessed content to make room for new entries. The ephemeral cache also automatically expires after 5 minutes, even if it hasn't reached capacity.

When making requests, Claude can "read" from the cache to reference previously cached content. However, the cache is ephemeral - it's not shared between different API calls or users. Each conversation thread has its own isolated cache.

Cost and Performance Benefits

Prompt caching offers significant advantages:
First request cost = normal pricing (includes cache creation)
Subsequent requests = reduced cost (only new content priced)
Response times improve because cached context is retrieved faster


> *(See full lesson at course URL)*

#### 56. Rules of prompt caching

Understanding how prompt caching works under the hood is essential for using it effectively. The caching system has specific rules about what gets cached, how the cache is managed, and what happens at boundaries between cached and uncached content.

Cache Structure and Organization

The prompt cache operates as a series of chunks that are stored and retrieved together. When you send a request with cached content, Claude stores these chunks sequentially in its cache. The key constraint is the 200K token maximum cache size.

Think of the cache as a series of storage blocks. When you make a request with multiple content blocks marked for caching, each block becomes a chunk in the cache. The total size of all chunks cannot exceed the maximum.

Block-Level Caching

Cache breaks occur when you don't mark content for caching. Each uncached block creates a boundary in the cache structure. This is important because:

Each cached block becomes its own chunk in the cache
Uncached blocks act as separators
You can't have multiple cached blocks in a single chunk

For example, if your conversation has:
Cached block 1 (system prompt) - becomes chunk 1
Uncached block 2 (user message) - creates a break
Cached block 3 (reference material) - becomes chunk 2

The cache now has two separate chunks rather than one continuous chunk.

Budget Management

The 200K token cache budget is shared across all cached chunks. When this budget is exhausted, older cached content gets evicted to make room for new content. The eviction follows least-recently-used semantics.

This means:
More recent cached content stays in cache longer
Older content gets pushed out first
If you exceed the budget, the oldest cached content is removed

The Cache Break Implication

Understanding cache breaks is crucial. When you don't cache a block, it creates a boundary that effectively "uses up" some of your budget. If you create many small cached blocks separated by uncached content, you consume more of your cache budget than necessary.

Efficient Structure

To maximize cache efficiency, consolidate your cached content into fewer, larger blocks rather than many small blocks. A single large cached block uses less cache budget than multiple smaller cached blocks that add up to the same total size.

Also consider that any content not marked for caching still gets processed by Claude. The cache is purely an optimization - uncached content still works exactly as it would without caching enabled.


> *(See full lesson at course URL)*

#### 57. Prompt caching in action

Prompt caching is a powerful optimization feature that makes your API requests both faster and cheaper when you're repeatedly sending the same content to Claude. Let's explore how to implement it effectively in your applications.

How Prompt Caching Works

When you enable prompt caching, the first request writes content to a cache that lives for one hour. Follow-up requests can then read from this cache instead of processing the same content again. This is particularly valuable when you're sending:

Large system prompts (like a 6K token coding assistant prompt)
Complex tool schemas (around 1.7K tokens for multiple tools)
Repeated message content

The key insight is that caching only helps if you're repeatedly sending identical content - but in many applications, this happens extremely frequently.

Setting Up Tool Schema Caching

To cache your tool schemas, you need to add a cache control field to the last tool in your list. Here's the proper way to do it without modifying your original tool definitions:

if tools:
    tools_clone = tools.copy()
    last_tool = tools_clone[-1].copy()
    last_tool["cache_control"] = {"type": "ephemeral"}
    tools_clone[-1] = last_tool
    params["tools"] = tools_clone

This approach creates copies of both the tools list and the last tool schema before adding the cache control field. While you could directly modify tools[-1]["cache_control"], the copying approach prevents issues if you later reorder your tools.

System Prompt Caching

For system prompts, you need to structure them as a text block with cache control:

if system:
    params["system"] = [
        {
            "type": "text",
            "text": system,
            "cache_control": {"type": "ephemeral"}
        }
    ]

This converts your system prompt from a simple string into a structured format that supports caching.

Understanding Cache Behavior

When you run requests with caching enabled, you'll see different usage patterns in the response:

First request: cache_creation_input_tokens=1772 - Claude writes to cache
Follow-up requests: cache_read_input_tokens=1772 - Claude reads from cache
Changed content: New cache creation tokens appear

The cache is extremely sensitive - changing even a single character in your tools or system prompt invalidates the entire cache for that component.

Cache Ordering and Breakpoints

You can set multiple cache breakpoints in a single request. The order matters:

Tools (if provided)
System prompt (if provided)
Messages


> *(See full lesson at course URL)*

#### 58. Code execution and the Files API

The Anthropic API offers two powerful features that work exceptionally well together: the Files API and Code Execution. While they might seem separate at first, combining them opens up some really interesting possibilities for delegating complex tasks to Claude.

Files API

The Files API provides an alternative way to handle file uploads. Instead of encoding images or PDFs directly in your messages as base64 data, you can upload files ahead of time and reference them later.

Here's how it works:

Upload your file (image, PDF, text, etc.) to Claude using a separate API call
Receive a file metadata object containing a unique file ID
Reference that file ID in future messages instead of including raw file data

This approach is particularly useful when you want to reference the same file multiple times or when working with larger files that would be cumbersome to include in every request.

Code Execution Tool

Code execution is a server-based tool that doesn't require you to provide an implementation. You simply include a predefined tool schema in your request, and Claude can optionally execute Python code in an isolated Docker container.

Key characteristics of the code execution environment:

Runs in an isolated Docker container
No network access (can't make external API calls)
Claude can execute code multiple times during a single conversation
Results are captured and interpreted by Claude for the final response

Combining Files API and Code Execution

The real power comes from using these features together. Since the Docker containers have no network access, the Files API becomes the primary way to get data in and out of the execution environment.

Here's a typical workflow:

Upload your data file (like a CSV) using the Files API
Include a container upload block in your message with the file ID
Ask Claude to analyze the data
Claude writes and executes code to process your file
Claude can generate outputs (like plots) that you can download

Practical Example

Let's look at a real example using streaming service data. The CSV file contains user information including subscription tiers, viewing habits, and whether they've churned (canceled their subscription).

First, upload the file using a helper function:

file_metadata = upload('streaming.csv')

Then create a message that includes both the uploaded file and a request for analysis:

messages = []
add_user_message(
    messages,
    [
        {
            "type": "text",

> *(See full lesson at course URL)*

#### 59. Quiz on features of Claude

video only


### 📖 Model Context Protocol

#### 60. Introducing MCP

Model Context Protocol (MCP) is a communication layer that provides Claude with context and tools without requiring you to write a bunch of tedious integration code. Think of it as a way to shift the burden of tool definitions and execution away from your server to specialized MCP servers.

When you first encounter MCP, you'll see diagrams showing the basic architecture: an MCP Client (your server) connecting to MCP Servers that contain tools, prompts, and resources. Each MCP server acts as an interface to some outside service.

Understanding MCP Through a Real Example

Let's say you're building a chat interface where users can ask Claude about their GitHub data. A user might ask "What open pull requests are there across all my repositories?" To answer this, Claude needs tools to access GitHub's API.

Without MCP, you'd need to create all the GitHub integration tools yourself. This means writing schemas and functions for every piece of GitHub functionality you want to support.

The Tool Function Problem

GitHub has massive functionality - repositories, pull requests, issues, projects, and much more. To build a complete GitHub chatbot, you'd need to author an incredible number of tools:

Each tool requires both a schema definition and a function implementation. This represents a lot of code that you have to write, test, and maintain as a developer.

How MCP Solves This

MCP shifts the burden of tool definitions and execution from your server to MCP servers. Instead of you writing all those GitHub tools, they're authored and executed inside a dedicated MCP server.

The MCP server acts as a wrapper around GitHub's functionality, providing pre-built tools that you can use without having to implement them yourself.

MCP servers provide access to data or functionality implemented by outside services. They package up complex integrations into reusable components that any application can connect to.

Common Questions About MCP

Who Authors MCP Servers?

Anyone can create an MCP server implementation. Often, service providers themselves will make their own official MCP implementations. For example, AWS might release an official MCP server with tools for their various services.

How is MCP Different from Direct API Calls?

MCP servers provide tool schemas and functions already defined for you. If you call an API directly, you're responsible for authoring those tool definitions yourself. MCP saves you that implementation work.

Isn't MCP Just Tool Use?


> *(See full lesson at course URL)*

#### 61. MCP clients

The MCP client serves as the communication bridge between your server and MCP servers. Think of it as your access point to all the tools that an MCP server provides. When you need to use external tools or services, the client handles all the message passing and protocol details for you.

Transport Agnostic Communication

One of MCP's key strengths is being transport agnostic - a fancy way of saying the client and server can talk to each other using different communication methods. The most common setup runs both the MCP client and server on the same machine, where they communicate through standard input/output.

But you're not limited to that approach. MCP clients and servers can also connect over:

HTTP
WebSockets
Various other network protocols

Message Types

Once connected, the client and server exchange specific message types defined in the MCP specification. The main message types you'll work with are:

ListToolsRequest/ListToolsResult: The client asks the server "what tools do you provide?" and gets back a list of available tools.

CallToolRequest/CallToolResult: The client asks the server to run a specific tool with certain arguments, then receives the results.

Complete Flow Example

Here's how all the pieces work together in a real scenario. Let's say a user asks "What repositories do I have?" - here's the complete communication flow:

The process starts when a user submits a query to your server. Your server realizes it needs to provide Claude with a list of available tools before making the request.

Your server asks the MCP client for tools, which sends a ListToolsRequest to the MCP server and receives a ListToolsResult back.

Now your server has everything needed to make the initial request to Claude - both the user's question and the available tools.

Claude examines the tools and decides it needs to call one to answer the question. It responds with a tool use request.

Your server asks the MCP client to execute the tool Claude requested. The MCP client sends a CallToolRequest to the MCP server, which then makes the actual request to GitHub.

GitHub returns the repository data, which flows back through the MCP server as a CallToolResult, then to the MCP client, and finally to your server.

Your server sends the tool results back to Claude in a follow-up message. Claude now has all the information it needs to formulate a complete response.

Finally, Claude responds with the formatted answer, which your server passes back to the user.


> *(See full lesson at course URL)*

#### 62. Project setup

We're going to build a CLI-based chatbot to better understand how MCP clients and servers work together. This hands-on project will give you practical experience with both sides of the MCP architecture.

What We're Building

Our chatbot will allow users to interact with a collection of documents through a command-line interface. The system consists of two main components:

An MCP client that handles user interactions
A custom MCP server that manages document operations

The server will provide two essential tools: one for reading document contents and another for updating them. All documents will be stored in memory for simplicity - no database required.

Important Architecture Note

In real-world projects, you typically implement either an MCP client or an MCP server, not both. You might create:

An MCP server to expose your service to other developers
An MCP client to connect to existing MCP servers

We're building both components in this project purely for educational purposes - to understand how they communicate and work together.

Project Setup

Download the cli_project.zip file attached to this lesson and extract it to your preferred development directory. Open your code editor in the project folder.

The project includes a comprehensive README file with setup instructions. Follow these steps:

Add your Anthropic API key to the .env file
Install dependencies using either UV (recommended) or pip
Run the starter application to verify everything works

Running the Application

Navigate to your project directory in the terminal. You'll see the main project files including main.py, mcp_client.py, and mcp_server.py.

To start the application, use one of these commands:

# If using UV (recommended)
uv run main.py

# If using standard Python
python main.py

When the application starts successfully, you'll see a chat prompt. Test it by asking a simple question like "what's 1+1?" - you should get a quick response from Claude.

With the basic setup complete, we're ready to start implementing MCP features and exploring how clients and servers communicate through the Model Control Protocol.

#### 63. Defining tools with MCP

Building an MCP server becomes much simpler when you use the official Python SDK. Instead of manually writing complex JSON schemas for tools, the SDK handles all that complexity for you with decorators and type hints.

In this example, we're creating an MCP server that manages documents stored in memory. The server will provide two essential tools: one to read document contents and another to update them through find-and-replace operations.

Setting Up the MCP Server

The Python MCP SDK makes server creation incredibly straightforward. You can initialize a complete MCP server with just one line:

from mcp.server.fastmcp import FastMCP
mcp = FastMCP("DocumentMCP", log_level="ERROR")

For this implementation, documents are stored in a simple Python dictionary where keys are document IDs and values contain the document content:

docs = {
    "deposition.md": "This deposition covers the testimony of Angela Smith, P.E.",
    "report.pdf": "The report details the state of a 20m condenser tower.",
    "financials.docx": "These financials outline the project's budget and expenditure",
    "outlook.pdf": "This document presents the projected future performance of the",
    "plan.md": "The plan outlines the steps for the project's implementation.",
    "spec.txt": "These specifications define the technical requirements for the equipment"
}

Tool Definition with Decorators

The SDK transforms tool creation from a verbose process into something clean and readable. Instead of writing lengthy JSON schemas, you use Python decorators and type hints.

Creating the Document Reader Tool

The first tool allows Claude to read any document by its ID. Here's the complete implementation:

@mcp.tool(
    name="read_doc_contents",
    description="Read the contents of a document and return it as a string."
)
def read_document(
    doc_id: str = Field(description="Id of the document to read")
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")
    return docs[doc_id]

The @mcp.tool decorator automatically generates the JSON schema that Claude needs. The Field class from Pydantic provides parameter descriptions that help Claude understand what each argument expects.

Building the Document Editor Tool

The second tool performs simple find-and-replace operations on documents:

@mcp.tool(
    name="edit_document",
    description="Edit a document by replacing a string in the documents content with a new string."
)
def edit_document(

> *(See full lesson at course URL)*

#### 64. The server inspector

When building MCP servers, you need a way to test your functionality without connecting to a full application. The Python MCP SDK includes a built-in browser-based inspector that lets you debug and test your server in real-time.

Starting the Inspector

First, make sure your Python environment is activated (check your project's README for the exact command). Then run the inspector with:

mcp dev mcp_server.py

This starts a development server on port 6277 and gives you a local URL to open in your browser. The inspector interface will load, showing the MCP Inspector dashboard.

Important Note About the Interface

The MCP inspector is actively being developed, so the interface you see might look different from current screenshots. However, the core functionality for testing tools, resources, and prompts should remain similar.

Connecting and Testing Tools

Click the Connect button on the left side to start your MCP server. Once connected, you'll see a navigation bar with sections for Resources, Prompts, Tools, and other features.

To test your tools:

Navigate to the Tools section
Click List Tools to see all available tools
Select a tool to open its testing interface
Fill in the required parameters
Click Run Tool to execute and see results

Testing Document Operations

For example, to test a document reading tool, you'd enter a document ID (like "deposition.md") and run the tool. The inspector shows the result, including any returned content or success messages.

You can chain operations to verify functionality. For instance, after editing a document by replacing text, you can immediately run the read tool again to confirm the changes were applied correctly.

Development Workflow

The inspector creates an efficient development loop:

Make changes to your MCP server code
Test individual tools through the inspector
Verify results without needing a full application setup
Debug issues in isolation

This tool becomes essential as you build more complex MCP servers. It eliminates the need to wire up your server to Claude or another application just to test basic functionality, making development much faster and more focused.

#### 65. Implementing a client

Now that we have our MCP server working, it's time to build the client side. The client is what allows our application to communicate with the MCP server and access its functionality.

Understanding the Client Architecture

In most real-world projects, you'll either implement an MCP client OR an MCP server - not both. We're building both in this project just so you can see how they work together.

The MCP client consists of two main components:

MCP Client - A custom class we create to make using the session easier
Client Session - The actual connection to the server (part of the MCP Python SDK)

The client session requires proper resource cleanup when we're done with it. That's why we wrap it in our custom MCP Client class - to handle all that cleanup automatically.

How the Client Fits Into Our Application

Our CLI code needs to do two main things with the MCP server:

Get a list of available tools to send to Claude
Execute tools when Claude requests them

The MCP client provides these capabilities through simple method calls that our application code can use.

Implementing the Core Methods

We need to implement two key methods in our client:

list_tools() - Gets all available tools from the server
call_tool() - Executes a specific tool on the server

List Tools Method

This method gets all available tools from the server:

async def list_tools(self) -> list[types.Tool]:
    result = await self.session().list_tools()
    return result.tools

It's straightforward - we access our session (the connection to the server), call the built-in list_tools() function, and return the tools from the result.

Call Tool Method

This method executes a specific tool on the server:

async def call_tool(
    self,
    tool_name: str,
    tool_input: dict
) -> types.CallToolResult | None:
    return await self.session().call_tool(tool_name, tool_input)

We pass the tool name and input parameters (provided by Claude) to the server and return the result.

Testing the Client

To test our implementation, we can run the client directly. The file includes a testing harness that connects to our MCP server and calls our methods:

async with MCPClient(
    command="uv",
    args=["run", "mcp_server.py"]
) as client:
    result = await client.list_tools()
    print(result)

When we run this test, we should see our tool definitions printed out, including the read_doc_contents and edit_document tools we created earlier.

Putting It All Together


> *(See full lesson at course URL)*

#### 66. Defining resources

Resources in MCP servers allow you to expose data to clients, similar to GET request handlers in a typical HTTP server. They're perfect for scenarios where you need to fetch information rather than perform actions.

Understanding Resources Through an Example

Let's say you want to build a document mention feature where users can type @document_name to reference files. This requires two operations:

Getting a list of all available documents (for autocomplete)
Fetching the contents of a specific document (when mentioned)

When a user types @, you need to show available documents. When they submit a message with a mention, you automatically inject that document's content into the prompt sent to Claude.

How Resources Work

Resources follow a request-response pattern. Your client sends a ReadResourceRequest with a URI, and the MCP server responds with the data. The URI acts like an address for the resource you want to access.

Types of Resources

There are two types of resources:

Direct Resources: Static URIs that don't change, like docs://documents
Templated Resources: URIs with parameters, like docs://documents/{doc_id}

For templated resources, the Python SDK automatically parses parameters from the URI and passes them as keyword arguments to your function.

Implementing Resources

Resources are defined using the @mcp.resource() decorator. Here's how to create both types:

Direct Resource (List Documents):

@mcp.resource(
    "docs://documents",
    mime_type="application/json"
)
def list_docs() -> list[str]:
    return list(docs.keys())

Templated Resource (Fetch Document):

@mcp.resource(
    "docs://documents/{doc_id}",
    mime_type="text/plain"
)
def fetch_doc(doc_id: str) -> str:
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")
    return docs[doc_id]

MIME Types

Resources can return any type of data - strings, JSON, binary, etc. The mime_type parameter gives clients a hint about what kind of data you're returning:

application/json - Structured JSON data
text/plain - Plain text content
Any other valid MIME type for different data formats

The MCP Python SDK automatically serializes your return values. You don't need to manually convert to JSON strings.

Testing Resources

You can test your resources using the MCP Inspector. Run your server with:

uv run mcp dev mcp_server.py

Then connect to the inspector in your browser. You'll see:

Resources: Lists your direct/static resources

> *(See full lesson at course URL)*

#### 67. Accessing resources

Resources in MCP allow your server to expose data that can be directly included in prompts, rather than requiring tool calls to access information. This creates a more efficient way to provide context to AI models like Claude.

Understanding Resource Requests

When you've defined resources on your MCP server, your client needs a way to request and use them. The client acts as a bridge between your application and the MCP server, handling the communication and data parsing automatically.

The flow is straightforward: when a user wants to reference a document (like typing "@report.pdf"), your application uses the MCP client to fetch that resource from the server and include its contents directly in the prompt sent to Claude.

Implementing Resource Reading

The core functionality requires a read_resource function in your MCP client. This function takes a URI parameter identifying which resource to fetch:

async def read_resource(self, uri: str) -> Any:
    result = await self.session().read_resource(AnyUrl(uri))
    resource = result.contents[0]

The response from the MCP server contains a contents list. You typically only need the first element, which contains the actual resource data along with metadata like the MIME type.

Handling Different Content Types

Resources can return different types of content, so your client needs to parse them appropriately. The MIME type tells you how to handle the data:

if isinstance(resource, types.TextResourceContents):
    if resource.mimeType == "application/json":
        return json.loads(resource.text)
    return resource.text

This approach ensures that JSON resources are properly parsed into Python objects, while plain text resources are returned as strings. The MIME type acts as your hint for determining the correct parsing strategy.

Required Imports

To make this work properly, you'll need these imports in your MCP client:

import json
from pydantic import AnyUrl

The json module handles parsing JSON responses, while AnyUrl ensures proper type handling for the URI parameter.

Testing Resource Access

Once implemented, you can test the functionality through your CLI application. When you type something like "What's in the @report.pdf document?", the system should:

Show available resources in an autocomplete list
Allow you to select a resource
Fetch the resource content automatically
Include that content in the prompt to Claude


> *(See full lesson at course URL)*

#### 68. Defining prompts

Prompts in MCP servers let you define pre-built, high-quality instructions that clients can use instead of writing their own prompts from scratch. Think of them as carefully crafted templates that give better results than what users might come up with on their own.

Why Use Prompts?

Let's say you want Claude to reformat a document into markdown. A user could just type "convert report.pdf to markdown" and it would work fine. But they'd probably get much better results with a thoroughly tested prompt that includes specific instructions about formatting, structure, and output requirements.

The key insight is that while users can accomplish these tasks on their own, they'll get more consistent and higher-quality results when using prompts that have been carefully developed and tested by the MCP server authors.

How Prompts Work

Prompts define a set of user and assistant messages that clients can use directly. When a client requests a prompt, your server returns a list of messages that can be sent straight to Claude.

The basic structure looks like this:

Define prompts using the @mcp.prompt() decorator
Add a name and description for each prompt
Return a list of messages that form the complete prompt
These prompts should be high quality, well-tested, and relevant to your MCP server's purpose

Building a Format Command

Here's how to implement a document formatting prompt. First, you'll need to import the base message types:

from mcp.server.fastmcp import base

Then define your prompt function:

@mcp.prompt(
    name="format",
    description="Rewrites the contents of the document in Markdown format."
)
def format_document(
    doc_id: str = Field(description="Id of the document to format")
) -> list[base.Message]:
    prompt = f"""Your goal is to reformat a document to be written with markdown syntax. The id of the document you need to reformat is: {doc_id}

Add in headers, bullet points, tables, etc as necessary. Feel free to add in extra formatting. Use the 'edit_document' tool to edit the document. After the document has been reformatted..."""
    return [
        base.UserMessage(prompt)
    ]

Testing Your Prompts

You can test prompts using the MCP Inspector. Navigate to the Prompts section, select your prompt, and provide any required parameters. The inspector will show you the generated messages that would be sent to Claude.


> *(See full lesson at course URL)*

#### 69. Prompts in the client

Prompts in MCP define a set of user and assistant messages that can be used by the client. These prompts should be high quality, well-tested, and relevant to the overall purpose of the MCP server.

Implementing List Prompts

The first step is implementing the list_prompts method in your MCP client. This method retrieves all available prompts from the server:

async def list_prompts(self) -> list[types.Prompt]:
    result = await self.session().list_prompts()
    return result.prompts

This simple implementation calls the session's list_prompts method and returns the prompts array from the result.

Getting Individual Prompts

The get_prompt method retrieves a specific prompt with arguments interpolated into it. When you request a prompt, you provide arguments that get passed to the prompt function as keyword arguments:

async def get_prompt(self, prompt_name, args: dict[str, str]):
    result = await self.session().get_prompt(prompt_name, args)
    return result.messages

The method returns the messages from the result, which form a conversation that can be fed directly into Claude.

How Prompt Arguments Work

When you define a prompt function on the server side, it can accept parameters. For example, a document formatting prompt might expect a doc_id parameter:

def format_document(doc_id: str):
    # The doc_id gets interpolated into the prompt

When the client calls get_prompt, the arguments dictionary should contain the expected keys. The MCP server will pass these as keyword arguments to the prompt function, allowing dynamic content to be inserted into the prompt template.

Testing Prompts in the CLI

Once implemented, you can test prompts through the command-line interface. When you type a forward slash, available prompts appear as commands. Selecting a prompt may prompt you to choose from available options (like document IDs), and then the complete prompt gets sent to Claude.

The workflow looks like this:

User selects a prompt (like "format")
System prompts for required arguments (like which document to format)
The prompt gets sent to Claude with the interpolated values
Claude can then use tools to fetch additional data and complete the task

Prompt Best Practices

When creating prompts for your MCP server:

Make them relevant to your server's purpose
Test them thoroughly before deployment
Use clear, specific instructions
Design them to work well with your available tools
Consider what arguments users will need to provide


> *(See full lesson at course URL)*

#### 70. MCP review

video only

#### 71. Quiz on Model Context Protocol

video only


### 📖 Anthropic apps - Claude Code and computer use

#### 72. Anthropic apps

In this module, we'll explore two powerful applications built by Anthropic: Claude Code and Computer Use. These aren't just useful tools on their own - they're perfect examples of AI agents in action. By understanding how they work, you'll get a solid foundation for building your own agents later.

Our Plan

We'll follow a progression that builds your understanding step by step:

Claude Code - Start with this agentic coding assistant that runs in your terminal
Computer Use - Explore this set of tools that lets Claude interact with desktop applications
Agents - Understand what makes these applications successful as agents

Claude Code

Claude Code is a terminal-based coding assistant that can help you with various programming tasks. Think of it as having Claude available right in your command line, ready to:

Edit files and fix bugs
Answer coding questions
Help with development workflows

We'll walk through the complete setup process and then use Claude Code on a small sample project so you can see exactly how it operates in practice.

Computer Use

Computer Use takes Claude's capabilities much further. It's a collection of tools that allow Claude to interact with a full desktop computer environment. This means Claude can:

Access websites and browse the internet
Interact with desktop applications
Perform tasks that require visual interface navigation

This dramatically expands what's possible compared to text-only interactions.

Why These Matter for Agents

Both Claude Code and Computer Use serve as excellent case studies for understanding agents. They demonstrate key principles that make agents effective:

Tool integration and usage
Multi-step task execution
Environmental interaction
Autonomous problem-solving

By examining these real-world implementations, you'll gain insights into what makes Claude Code and Computer Use successful, which will inform your own agent development work.

Let's start with the setup process for Claude Code in the next section.

#### 73. Claude Code setup

Claude Code is a terminal-based coding assistant that runs directly in your command line. Think of it as having Claude available right in your terminal to help with any coding task you're working on.

What Claude Code Can Do

Claude Code comes with a comprehensive set of tools to help with your development workflow:

File operations - Search, read, and edit files in your project
Terminal access - Run commands directly from the conversation
Web access - Search documentation, fetch code examples, and more
MCP Server support - Add additional tools by connecting MCP servers

The MCP integration is particularly powerful because it means you can extend Claude Code's capabilities by adding specialized tools for databases, APIs, or any other services you work with.

Claude Code works across MacOS, Windows WSL, and Linux, so you can use it regardless of your development environment.

Installation

Getting Claude Code set up takes just three steps:

Install Node.js from nodejs.org/en/download (check if you already have it by running npm help in your terminal)
Install Claude Code with the command: npm install -g @anthropic-ai/claude-code
Start and login by running claude in your terminal

When you run the claude command for the first time, it will prompt you to log in to your Anthropic account. The full setup guide is available at docs.anthropic.com if you need more detailed instructions.

Once you're set up, you'll have Claude available directly in your terminal, ready to help with any coding project or task you're working on.

#### 74. Claude Code in action

Claude Code isn't just a tool for writing code - it's designed to work alongside you throughout every phase of a software project. Think of it as another engineer on your team who can handle everything from initial setup to deployment and support.

The /init Command

When you start working with Claude Code on a project, the first thing you'll want to do is run the /init command. This tells Claude to scan your entire codebase and understand your project's structure, dependencies, coding style, and architecture.

Claude summarizes everything it learns in a special file called CLAUDE.md. This file automatically gets included as context in all future conversations, so Claude remembers important details about your project.

You can have multiple CLAUDE.md files for different scopes:

Project - Shared between all engineers working on the project
Local - Your personal notes that aren't checked into git
User - Used across all your projects

When running /init, you can add special directions for areas you want Claude to focus on. The generated file will include build commands, coding guidelines, and project-specific patterns that Claude should follow.

You can also quickly add notes to your CLAUDE.md file using the # command. For example, typing # Always use descriptive variable names will prompt you to add this guideline to your project, local, or user memory.

Common Workflows

Claude works best when you approach it as an effort multiplier. The more context and structure you provide, the better results you'll get.

Step 1: Feed Context into Claude

Before asking Claude to build something, identify files in your codebase that are relevant to the feature you want to create. Ask Claude to read and analyze these files first. This gives Claude examples of your coding patterns and existing functionality it can build upon.

Step 2: Tell Claude to Plan a Solution

Instead of jumping straight to implementation, ask Claude to think through the problem and create a plan. Tell Claude specifically not to write any code yet - just focus on the approach and steps needed.

Step 3: Ask Claude to Implement the Solution

Once you have a solid plan, ask Claude to implement it. Claude will write code based on the context and planning work you've already done together.

Test-Driven Development Workflow

For even better results, you can use a test-driven approach:

Feed context into Claude - Same as before, show Claude relevant files

> *(See full lesson at course URL)*

#### 75. Enhancements with MCP servers

Claude Code has an MCP client built right into it, which means you can connect MCP servers to dramatically expand what Claude can do. This opens up some really powerful possibilities for customizing your development workflow.

How MCP Extends Claude

The Model Context Protocol allows Claude Code to connect to external services and tools through MCP servers. Instead of being limited to Claude's built-in capabilities, you can add custom functionality by connecting servers that provide specific tools, resources, or integrations.

Each MCP server can expose different types of functionality to Claude through three main components: Tools (for taking actions), Prompts (for templates), and Resources (for accessing data).

Setting Up an MCP Server

Adding an MCP server to Claude Code is straightforward. You use the command line to register your server:

claude mcp add [server-name] [command-to-start-server]

For example, if you have a document processing server that starts with uv run main.py, you'd run:

claude mcp add documents uv run main.py

Once registered, Claude Code will automatically connect to your server when it starts up.

Example: Document Processing

A practical example is creating a tool that lets Claude read PDF and Word documents. By building an MCP server with a document_path_to_markdown tool, you can ask Claude to convert document contents to markdown format.

When you ask Claude to "Convert the tests/fixtures/mcp_docs.docx file to markdown", it will automatically use your custom tool to read the document and return the converted content.

Popular MCP Integrations

The MCP ecosystem includes servers for many common development tools and services:

sentry-mcp - Automatically discover and fix bugs logged in Sentry
playwright-mcp - Gives Claude browser automation capabilities for testing and troubleshooting
figma-context-mcp - Exposes Figma designs to Claude
mcp-atlassian - Allows Claude to access Confluence and Jira
firecrawl-mcp-server - Adds web scraping capabilities to Claude
slack-mcp - Allows Claude to post messages or reply to specific threads

Building Your Development Workflow

The real power comes from combining multiple MCP servers that match your specific development process. You might set up:

A Sentry server to fetch production error details
A Jira server to read ticket requirements
A Slack server to notify your team when work is complete
Custom servers for your internal tools and APIs


> *(See full lesson at course URL)*


### 📖 Agents and workflows

#### 76. Agents and workflows

Workflows and agents are strategies for handling user tasks that can't be completed by Claude in a single request. You've actually been creating both throughout this course - when you used tools and let Claude figure out how to complete tasks, that was an agent.

When to Use Workflows vs Agents

The decision comes down to how well you understand the task:

Use workflows when you can picture the exact flow or steps that Claude should go through to solve a problem, or when your app's UX constrains users to a set of tasks
Use agents when you're not sure exactly what task or task parameters you'll give to Claude

Workflows are a series of calls to Claude meant to solve a specific problem through a predetermined series of steps. Agents give Claude a goal and a set of tools, expecting Claude to figure out how to complete the goal through the provided tools.

Example: Image to CAD Workflow

Let's look at a practical workflow example. Imagine building a web app where users drag and drop an image of a metal part, and you create a STEP file (an industry standard for 3D models) from it.

Since we have a pretty good idea of exactly what to do when a user supplies an image file, and we can easily write all of this out with code as a predefined series of steps, this makes a perfect workflow candidate.

Here's how the workflow breaks down:

Feed an image into Claude, asking it to describe the object
Based on the description, ask Claude to use the CadQuery library to model the object
Create a rendering
Ask Claude to grade the rendering against the original image. If there are issues, fix them

The Evaluator-Optimizer Pattern

This modeling workflow is an example of an evaluator-optimizer pattern. Here's how it works:

Producer: Takes input and creates output (Claude using CadQuery to model the part and create a rendering)
Grader: Evaluates the output against some criteria
Feedback loop: If the grader doesn't accept the output, feedback goes back to the producer for improvement
Iteration: The cycle repeats until the grader accepts the output

Why Learn Workflow Patterns

The goal of identifying different workflows is to give you a set of repeatable recipes for implementing your own features. The Evaluator-Optimizer is one workflow pattern that has worked well for other engineers - consider using it in your own app!


> *(See full lesson at course URL)*

#### 77. Parallelization workflows

When building AI applications, you'll often encounter tasks that seem simple on the surface but become complex when you try to implement them effectively. Let's explore a powerful pattern called parallelization workflows that can help you break down complex tasks into manageable, focused pieces.

The Problem with Complex Single Prompts

Imagine you're building a material designer application where users upload images of parts and receive recommendations for the best material to use. Your first instinct might be to send the image to Claude with a simple prompt asking it to choose between metal, polymer, ceramic, composite, elastomer, or wood.

While this approach might work, you're asking Claude to do a lot of heavy lifting in a single request. Without specific criteria for each material type, the results won't be as reliable as they could be.

You might think to improve this by adding detailed criteria for each material into one massive prompt. But this creates a new problem - Claude has to juggle all these different considerations simultaneously, which can lead to confusion and suboptimal results.

A Better Approach: Parallelization

Instead of cramming everything into one request, you can split the task into multiple parallel requests. Each request focuses on evaluating the part for a single material type with specialized criteria.

Here's how it works:

Send the same image to Claude multiple times simultaneously
Each request includes specialized criteria for one material (metal criteria, polymer criteria, ceramic criteria, etc.)
Claude evaluates the part's suitability for each material independently
Collect all the analysis results and feed them into a final aggregation step

The final step sends all the individual analysis results back to Claude with a request to compare them and make a final material recommendation.

How Parallelization Workflows Work

The parallelization pattern follows a simple structure:

Split a single task into multiple sub-tasks - Break down the complex decision into focused, specialized evaluations
Run the sub-tasks in parallel - Execute all evaluations simultaneously for faster processing
Aggregate the results together - Combine the specialized analyses into a final decision
The parallelized sub-tasks don't need to be identical - Each can have a specialized prompt, set of tools, or evaluation criteria

Benefits of This Approach

Parallelization workflows offer several key advantages:


> *(See full lesson at course URL)*

#### 78. Chaining workflows

Chaining workflows might seem obvious at first, but they're actually one of the most useful patterns you'll encounter when working with Claude. This approach becomes especially valuable when you're dealing with complex tasks or long prompts that Claude struggles to handle consistently.

What is Workflow Chaining?

A chaining workflow breaks down a large, complex task into smaller, sequential subtasks. Instead of asking Claude to do everything at once, you split the work into focused steps that build on each other.

Here's a practical example: imagine you're building a social media marketing tool that creates and posts videos automatically. Rather than asking Claude to handle everything in one massive prompt, you could break it down like this:

Find related trending topics on Twitter
Select the most interesting topic (using Claude)
Research the topic (using Claude)
Write a script for a short format video (using Claude)
Use an AI avatar and text-to-speech to create a video
Post the video to social media

Why Chain Instead of One Big Prompt?

You might wonder why not just combine all the Claude tasks into a single prompt. The key benefit is focus - when you give Claude one specific task at a time, it can concentrate on doing that task well rather than juggling multiple requirements simultaneously.

The chaining approach offers several advantages:

Split large tasks into smaller, non-parallelizable subtasks
Optionally do non-LLM processing between each task
Keep Claude focused on one aspect of the overall task

The Long Prompt Problem

Here's where chaining becomes really valuable. You'll often encounter situations where you need Claude to write content with many specific constraints. Let's say you want Claude to write a technical article, and you specify that it should:

Not mention that it's written by an AI
Avoid using emojis
Skip clichéd or overly casual language
Write in a professional, technical tone

Even with all these constraints clearly stated, Claude might still produce content that violates some of your rules. You might get back an article that still uses emojis, mentions AI authorship, or sounds unprofessional.

The Chaining Solution

Instead of fighting with one massive prompt, use a two-step chaining approach:

Step 1: Send your initial prompt and accept that the first result might not be perfect. Claude will generate an article, but it might violate some of your constraints.


> *(See full lesson at course URL)*

#### 79. Routing workflows

Routing workflows let you intelligently direct user requests to the most appropriate workflow or processing path based on the input. This is one of the most powerful patterns for building flexible, scalable AI applications.

The Core Idea

At its simplest, routing means taking a user's input and deciding which workflow should handle it. This becomes particularly powerful when combined with classifier models that can analyze the input and determine the best approach.

Consider a customer support chatbot. Instead of having one generic workflow that tries to handle everything, you could route requests to specialized workflows:
Technical troubleshooting gets routed to a workflow that knows how to diagnose problems
Billing questions go to a workflow that can look up accounts and process refunds
Product questions route to a workflow with detailed product documentation

This specialized approach gives much better results than trying to handle everything with a single, generic workflow.

Classifier-Based Routing

The most sophisticated routing uses a classifier model to analyze the input and decide where it should go. This classifier can be a simple heuristic-based system, or it can be another AI model entirely.

For example, you might use Claude itself as a classifier:

Send the user query to Claude with instructions to classify it
Claude analyzes the input and determines the best category
Based on the classification, your application routes to the appropriate workflow

This approach is powerful because Claude can understand nuanced requests and make sophisticated routing decisions.

When Routing Excels

Routing workflows are particularly effective when:

You have distinct, well-defined categories of inputs that need different handling
Some inputs require specialized tools or knowledge
You want to provide focused, expert responses rather than generic ones
Your application needs to scale across many different use cases

The key is identifying where the paths diverge in your application. If you find yourself building increasingly complex conditional logic to handle different types of requests, that's often a sign that routing would serve you better.

Combining with Other Patterns

Routing doesn't exist in isolation. It's often combined with other workflow patterns:

Parallelization: Different routed paths might spawn parallel sub-tasks
Chaining: Each routed path might involve its own chain of steps

> *(See full lesson at course URL)*

#### 80. Agents and tools

Agents represent a powerful paradigm where you give Claude a goal and a set of tools, then let Claude figure out how to accomplish that goal through the provided tools. This is fundamentally different from workflows, where you define the exact steps to take.

The Key Difference

With workflows, you know the path to the solution. With agents, you're delegating problem-solving to Claude, trusting it to determine the right approach.

Let's explore what makes agents work well through a practical example: a document analysis agent that can search for files, read documents, analyze content, and create summaries.

Setting Up an Agent

An agent needs three core components:

A goal or objective - What it's trying to accomplish
Tools - Capabilities it can use to accomplish the goal
Autonomy - The freedom to decide how to achieve the goal

For our document analysis agent, we might provide tools to:
Search for files by name or content
Read document contents
Analyze and extract key information
Generate summaries or reports

The Loop of Agentic Behavior

Agents operate in a loop: observe the current state, think about what to do next, take action, then repeat until the goal is achieved. This loop continues until either the task is complete or a maximum number of iterations is reached.

The observe step is crucial. For a document agent, this might mean checking what files are available, reading search results, or examining document contents. The think step involves deciding what to do with that information. The act step is where the actual tool calls happen.

When Agents Work Best

Agents shine in scenarios where:

You can't easily predict all the steps needed upfront
The task involves exploration or discovery
Multiple tools need to be combined in flexible ways
The path to the solution isn't predetermined

For example, a research agent that needs to answer a complex question might need to search for relevant documents, read several of them, extract key information, and synthesize findings. The exact sequence depends on what it discovers at each step.

Building Effective Agents

The key to building effective agents is providing the right tools and clear objectives. The tools determine what the agent can do. The objective tells the agent what success looks like.

Be specific about what you want the agent to accomplish. Vague objectives lead to unpredictable results. The more clearly you define success criteria, the better the agent can work toward that goal.


> *(See full lesson at course URL)*

#### 81. Environment inspection

When building AI agents, one crucial concept often gets overlooked: environment inspection. Claude operates blindly - it needs to be able to observe and understand the results of its actions to work effectively.

Why Environment Inspection Matters

Think about how Claude works with computer use. Every time Claude performs an action like typing text or clicking a button, it immediately receives a screenshot to understand what happened. This isn't just a nice-to-have feature - it's essential.

From Claude's perspective, clicking a button could navigate to a new page, open a menu, or trigger any number of changes. Without being able to see the results, Claude has no way to understand whether its action succeeded or what the new state of the environment looks like.

Reading Before Writing

This same principle applies to file operations. Before Claude can modify any file, it needs to understand the current contents. This might seem obvious, but it's a pattern you should always follow when building agents.

When asked to add a new route to a Python file, Claude first reads the existing code to understand the current structure. Only then can it safely make the requested changes without breaking existing functionality.

System Prompts for Environment Inspection

You can guide Claude to inspect its environment through system prompts. For complex tasks like video generation, this becomes especially important.

Consider a video creation agent that needs to:

Generate video content using tools like FFmpeg
Verify that audio dialogue is placed correctly
Check that visual elements appear as expected

You might include system prompt instructions like:

Use the bash tool to run whisper.cpp and generate caption files with timestamps to verify dialogue placement
Use FFmpeg to extract screenshots from the video at regular intervals to visually inspect the output
Compare the generated content against the original requirements

Benefits of Environment Inspection

When Claude can inspect its environment, several things improve:

Better progress tracking - Claude can gauge how close it is to completing a task
Error handling - Unexpected results can be detected and corrected
Quality assurance - Output can be verified before considering a task complete
Adaptive behavior - Claude can adjust its approach based on what it observes

Practical Implementation


> *(See full lesson at course URL)*

#### 82. Workflows vs agents

Understanding when to use workflows versus agents is crucial for building effective AI applications. Both approaches extend Claude beyond single requests, but they serve different purposes and excel in different situations.

Workflows: Defined Paths to Solutions

Workflows are ideal when you understand the path to solving a problem. You define the exact steps Claude should take, creating a predetermined sequence of operations.

Use workflows when:

You can articulate the specific steps needed to solve the problem
The solution path is predictable and well-defined
Your application UX constrains users to specific task types
You need consistent, reproducible processing steps

For example, a document conversion workflow has a clear path: read the input file, transform it according to rules, output the result. You know the steps, so you encode them as a workflow.

Agents: Delegating Problem-Solving

Agents work best when you know the goal but not necessarily the path. You give Claude tools and objectives, then trust it to determine how to accomplish the task.

Use agents when:

The task involves exploration or discovery
Multiple tools need flexible combination
You cannot predict all the steps upfront
The path to the solution varies based on intermediate results

For instance, a research agent might need to search for papers, read relevant ones, extract key findings, and synthesize results. The exact sequence depends on what it discovers at each step.

Key Decision Framework

Ask yourself: do I know the path to the solution?

If yes: Use a workflow. Define the steps and let Claude execute them systematically.

If no: Use an agent. Provide tools and objectives and let Claude figure out the approach.

Often, hybrid approaches work best. Use workflows for well-understood parts of a process, then deploy agents for sections requiring flexibility and adaptation.

Practical Application

When designing your AI application, start by analyzing each user request type:

Which request types have predictable, well-defined solution paths?
Which require flexibility and adaptive behavior?
Can complex tasks be decomposed into workflow steps with agent-like flexibility at certain points?

Build your application to match the nature of each task type. Don't force everything into one paradigm when a combination serves better.

#### 83. Quiz on Agents and Workflows

video only


### 📖 Final assessment

#### 84. Final Assessment

video only


### 📖 Wrapping up!

#### 85. Course Wrap Up

Congratulations on completing the Building with the Claude API course! You've learned about: the Claude API, prompt engineering techniques, tool use, RAG systems, advanced Claude features, the Model Context Protocol, and agents and workflows. Thank you for taking the time to learn with us. We hope these skills help you build amazing applications. Good luck!


---

*Summary generated from course content at https://anthropic.skilljar.com/claude-with-the-anthropic-api*
