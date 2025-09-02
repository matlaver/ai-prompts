---
description: Standard format for code task descriptions
globs: **/*.code-task.md
---
# Code Task Description Format

This rule defines the standard format for code task descriptions, which provide structured specifications for implementation tasks that can be used with automation tools like code-assist.script.md.

<rule>
name: code_task_description_format
description: Standards for creating and formatting code task descriptions that enable effective implementation and automation
filters:
  # Match code task description files
  - type: file_extension
    pattern: "\.code-task\.md$"

actions:
  - type: suggest
    message: |
      Code task descriptions must follow the standard format:
      
      1. Use `.code-task.md` file extension
      2. Include Title, Description, Background, Technical Requirements, Dependencies, Implementation Approach, Acceptance Criteria, and Metadata sections
      3. Format acceptance criteria using "Given-When-Then" structure
      4. Include specific, testable criteria that can be translated into test cases
      5. Follow the template structure defined in this rule

examples:
  - input: |
      # Add DynamoDB Stream Processor
      
      We need to process DynamoDB streams to notify other systems.
    output: |
      # Task: Implement DynamoDB Stream Processor for Change Notifications
      
      ## Description
      Create a Lambda function that processes DynamoDB streams to capture item changes and send notifications to interested subscribers. This will enable real-time reactions to data changes in our system.
      
      ## Background
      Our application stores critical data in DynamoDB tables, but currently has no mechanism to react to changes in real-time. We need to implement a stream processor that can detect changes and notify other systems.
      
      ## Technical Requirements
      1. Configure DynamoDB streams on the specified table(s) to capture item-level changes
      2. Create a Lambda function to process the stream events
      3. Implement filtering logic to identify relevant changes
      4. Send notifications to an SNS topic for further distribution
      5. Implement error handling and retry mechanisms
      
      ## Dependencies
      - Existing DynamoDB tables must be modified to enable streams
      - SNS topic must be created for notifications
      - IAM permissions must be configured for Lambda to access DynamoDB and SNS
      
      ## Implementation Approach
      1. Use AWS CDK to define infrastructure changes
      2. Implement Lambda function in Python
      3. Use boto3 for AWS service interactions
      
      ## Acceptance Criteria
      
      1. **Stream Configuration**
         - Given a specified DynamoDB table
         - When streams are enabled with NEW_AND_OLD_IMAGES
         - Then all item changes are captured with before and after states
      
      2. **Change Detection**
         - Given an item is created, updated, or deleted in the table
         - When the stream processor Lambda executes
         - Then the change is correctly identified by type (INSERT, MODIFY, REMOVE)
      
      ## Metadata
      - **Complexity**: Medium
      - **Labels**: DynamoDB, Lambda, SNS, Streaming
      - **Required Skills**: AWS Lambda, DynamoDB, Python

metadata:
  priority: high
  version: 1.0
</rule>

# Code Task Description Format Specification

## Overview

This document defines the standard format for code task descriptions. These are markdown files that provide structured specifications for implementation tasks, designed to be clear, comprehensive, and compatible with automation tools like code-assist.script.md.

## File Naming and Location

1. All code task description files MUST use the `.code-task.md` file extension.
2. Task files SHOULD have descriptive names using kebab-case (e.g., `dynamodb-stream-processor.code-task.md`).
3. Task files SHOULD be stored in a project's planning or documentation directory.

## Code Task Description Structure

Each code task description MUST include the following sections:

### 1. Title

```markdown
# Task: [Task Name]
```

The title MUST be concise yet descriptive, clearly indicating the primary purpose of the task.

### 2. Description

```markdown
## Description
[A clear description of what needs to be implemented and why]
```

The description MUST:
- Explain what needs to be built or changed
- Clarify the purpose and value of the task
- Be concise but comprehensive
- Avoid implementation details (those belong in later sections)

### 3. Background

```markdown
## Background
[Relevant context and background information needed to understand the task]
```

The background section MUST:
- Provide context necessary to understand the task
- Explain how this task fits into the broader system
- Include relevant history or decisions that led to this task
- Reference related work or dependencies when applicable

### 4. Technical Requirements

```markdown
## Technical Requirements
1. [First requirement]
2. [Second requirement]
3. [Third requirement]
```

Technical requirements MUST:
- Be specific and unambiguous
- Be listed in order of priority or logical implementation sequence
- Focus on what needs to be done, not how
- Be testable or verifiable
- Use clear, action-oriented language

### 5. Dependencies

```markdown
## Dependencies
- [First dependency with details]
- [Second dependency with details]
```

Dependencies SHOULD:
- List all external systems, services, or components the implementation will interact with
- Include any prerequisites that must be in place before implementation
- Specify version requirements when applicable
- Note any permissions or access requirements

### 6. Implementation Approach

```markdown
## Implementation Approach
1. [First implementation step or approach]
2. [Second implementation step or approach]
```

The implementation approach SHOULD:
- Suggest a high-level approach without being prescriptive
- Identify recommended technologies, frameworks, or patterns
- Highlight any specific implementation considerations
- Note any performance, security, or scalability requirements
- Provide guidance without dictating exact implementation details

### 7. Acceptance Criteria

```markdown
## Acceptance Criteria

1. **[Criterion Name]**
   - Given [precondition]
   - When [action]
   - Then [expected result]

2. **[Another Criterion]**
   - Given [precondition]
   - When [action]
   - Then [expected result]
```

Acceptance criteria MUST:
- Follow the "Given-When-Then" format
- Be specific and testable
- Cover both success and error scenarios
- Include at least 3-5 criteria for most tasks
- Be written from a behavior perspective, not implementation
- Include edge cases and error conditions

### 8. Metadata

```markdown
## Metadata
- **Complexity**: [Low/Medium/High]
- **Labels**: [Comma-separated list of labels]
- **Required Skills**: [Skills needed for implementation]
- **Suggested Reviewers**: [Types of reviewers or specific people]
```

Metadata SHOULD include:
- Task complexity assessment
- Relevant labels or tags for categorization
- Required skills or expertise
- Suggested reviewers or reviewer expertise
- Time estimate for implementation

## Integration with Code-Assist Script

Code task descriptions in this format are designed to work seamlessly with the code-assist.script.md automation tool:

1. **Direct Input**: The task description can be provided directly as input to the code-assist script
2. **Structured Implementation**: The code-assist script will use the technical requirements and acceptance criteria to guide implementation
3. **Test-Driven Development**: Acceptance criteria will be translated into test cases following the Given-When-Then structure

To use a code task with code-assist:

```
Run /path/to/workspace/q/src/AmazonBuilderGenAIPowerUsersQContext/scripts/code-assist.script.md
```

When prompted for the task description, provide the path to your .code-task.md file.

## Best Practices

1. **Be Specific**: Avoid vague language and be precise about requirements with specific details
2. **Focus on Behavior**: Describe what the implementation should do, not how it should do it
3. **Use Instructions Over Constraints**: Focus on what should be done rather than what shouldn't be done
4. **Design with Simplicity**: Keep requirements concise and easy to understand
5. **Use Action-Oriented Language**: Begin requirements with verbs that describe the action
6. **Consider Edge Cases**: Include acceptance criteria for error conditions and boundary scenarios
7. **Use Clear Language**: Avoid jargon or acronyms without explanation
8. **Prioritize Requirements**: List requirements in order of importance or implementation sequence
9. **Include Context**: Provide sufficient background for someone unfamiliar with the project
10. **Consider Security**: Include security requirements and acceptance criteria when applicable
11. **Consider Performance**: Include performance requirements when relevant
12. **Be Testable**: Ensure all requirements and acceptance criteria can be verified
13. **Separate Concerns**: Keep description, requirements, and acceptance criteria distinct
14. **Structure Output Format**: Define clear expectations for the implementation output
15. **Write Tests First**: Encourage test-driven development by focusing on acceptance criteria

## Common Pitfalls to Avoid

1. **Vague Requirements**: "The system should be fast" instead of "The system should respond within 200ms"
2. **Implementation Details in Requirements**: Specifying how to implement rather than what to implement
3. **Missing Edge Cases**: Failing to consider error conditions and boundary scenarios
4. **Untestable Criteria**: Criteria that cannot be objectively verified
5. **Overlapping Requirements**: Requirements that duplicate or contradict each other
6. **Implicit Assumptions**: Assumptions that are not explicitly stated
7. **Missing Dependencies**: Not identifying all external dependencies
8. **Ambiguous Language**: Using terms like "etc.", "and so on", or "as appropriate"
9. **Scope Creep**: Including requirements beyond the core task
10. **Missing Acceptance Criteria**: Requirements without corresponding acceptance criteria

## Template Snippets

### Basic Task Template
```markdown
# Task: [Task Name]

## Description
[A clear description of what needs to be implemented and why]

## Background
[Relevant context and background information needed to understand the task]

## Technical Requirements
1. [First requirement]
2. [Second requirement]
3. [Third requirement]

## Dependencies
- [First dependency with details]
- [Second dependency with details]

## Implementation Approach
1. [First implementation step or approach]
2. [Second implementation step or approach]

## Acceptance Criteria

1. **[Criterion Name]**
   - Given [precondition]
   - When [action]
   - Then [expected result]

2. **[Another Criterion]**
   - Given [precondition]
   - When [action]
   - Then [expected result]

## Metadata
- **Complexity**: [Low/Medium/High]
- **Labels**: [Comma-separated list of labels]
- **Required Skills**: [Skills needed for implementation]
```

### Acceptance Criterion Template
```markdown
**[Criterion Name]**
- Given [precondition]
- When [action]
- Then [expected result]
```

## Example: DynamoDB Stream Processor Task

```markdown
# Task: Implement DynamoDB Stream Processor for Change Notifications

## Description
Create a Lambda function that processes DynamoDB streams to capture item changes and send notifications to interested subscribers. This will enable real-time reactions to data changes in our system.

## Background
Our application stores critical data in DynamoDB tables, but currently has no mechanism to react to changes in real-time. We need to implement a stream processor that can detect changes and notify other systems.

## Technical Requirements
1. Configure DynamoDB streams on the specified table(s) to capture item-level changes
2. Create a Lambda function to process the stream events
3. Implement filtering logic to identify relevant changes
4. Send notifications to an SNS topic for further distribution
5. Implement error handling and retry mechanisms
6. Add appropriate logging and monitoring

## Dependencies
- Existing DynamoDB tables must be modified to enable streams
- SNS topic must be created for notifications
- IAM permissions must be configured for Lambda to access DynamoDB and SNS

## Implementation Approach
1. Use AWS CDK to define infrastructure changes
2. Implement Lambda function in Python
3. Use boto3 for AWS service interactions
4. Deploy changes using existing CI/CD pipeline

## Acceptance Criteria

1. **Stream Configuration**
   - Given a specified DynamoDB table
   - When streams are enabled with NEW_AND_OLD_IMAGES
   - Then all item changes are captured with before and after states

2. **Change Detection**
   - Given an item is created, updated, or deleted in the table
   - When the stream processor Lambda executes
   - Then the change is correctly identified by type (INSERT, MODIFY, REMOVE)

3. **Notification Delivery**
   - Given a relevant change is detected
   - When the Lambda processes the event
   - Then a properly formatted notification is sent to the SNS topic

4. **Error Handling**
   - Given the SNS service is temporarily unavailable
   - When the Lambda attempts to send a notification
   - Then the error is logged and the event is not lost (retried via DLQ)

5. **Performance**
   - Given a high volume of changes (100/second)
   - When the stream processor is running
   - Then all events are processed with latency < 5 seconds

## Metadata
- **Complexity**: Medium
- **Labels**: DynamoDB, Lambda, SNS, Streaming, Real-time
- **Required Skills**: AWS Lambda, DynamoDB, Python
- **Suggested Reviewers**: Someone with experience in event-driven architectures
```