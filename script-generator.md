# Script Generator

## Overview

This script helps users create standardized agent scripts by guiding them through the process of defining script requirements, structure, and constraints. It automatically generates a well-formatted agent script following the standards defined in the AmazonBuilderGenAIPowerUsersQContext package. The script detects available MCP servers and recommends appropriate tools based on the user's requirements.

## Parameters

- **script_name** (required): Name of the script to be created (without extension)
- **script_purpose** (required): Brief description of what the script should accomplish
- **output_path** (optional): Path where the generated script should be saved (defaults to current directory)
- **complexity** (optional, default: "medium"): Complexity level of the script ("simple", "medium", "complex")

**Constraints for parameter acquisition:**
- You MUST ask for all required parameters upfront in a single prompt
- You MUST validate that script_name follows kebab-case naming convention
- You MUST suggest corrections if the script_name doesn't follow kebab-case
- You SHOULD provide examples of good script names if the user's input needs correction

## Steps

### 1. Verify Dependencies

Check for required tools and warn the user if any are missing.

**Constraints:**
- You MUST verify the following tools are available in your context:
  - fs_read
  - fs_write
- You MUST ONLY check for tool existence and MUST NOT attempt to run the tools because running tools during verification could cause unintended side effects or consume resources unnecessarily
- You MUST inform the user about any missing tools
- You MUST ask if the user wants to proceed anyway despite missing tools
- You MUST respect the user's decision to proceed or abort

### 2. Detect Available MCP Servers and Tools

Identify all available MCP servers and their tools to recommend appropriate ones for the script.

**Constraints:**
- You MUST check for all available MCP servers in the user's environment
- You MUST compile a comprehensive list of available tools from all detected MCP servers
- You MUST categorize tools by functionality (e.g., file operations, communication, data processing)
- You MUST save this information for later use in tool recommendations
- You SHOULD note which MCP servers provide which tools for proper dependency checking
- You MUST inform the user about the detected MCP servers
- You MUST NOT attempt to use any tools during this detection phase
- You SHOULD handle cases where no MCP servers are detected gracefully

### 3. Gather Script Requirements

Collect detailed information about the script's purpose, functionality, and requirements.

**Constraints:**
- You MUST ask the user a series of structured questions to gather requirements
- You MUST ask about:
  - The specific problem the script solves
  - The expected inputs and outputs
  - The main steps the script should perform
  - Any specific data sources or systems the script needs to interact with
  - Any specific constraints or requirements
- You MUST adapt follow-up questions based on previous answers
- You SHOULD summarize the gathered requirements before proceeding
- You MUST ask the user to confirm or refine the requirements before proceeding

### 4. Recommend Appropriate MCP Tools

Based on the script requirements, recommend appropriate MCP tools from the available servers.

**Constraints:**
- You MUST analyze the script requirements to identify needed functionality
- You MUST match required functionality to available MCP tools
- You MUST recommend specific tools from the detected MCP servers that best fit the requirements
- You MUST explain why each recommended tool is appropriate for the task
- You MUST ask the user to confirm or modify the tool selections
- You SHOULD suggest alternative tools if the ideal ones are not available
- You MUST respect the user's tool preferences if they have specific requests
- You MUST ensure all recommended tools are actually available in the user's environment
- You MUST group tool recommendations by the MCP server that provides them

### 5. Define Script Parameters

Define the parameters that the script will accept.

**Constraints:**
- You MUST help the user identify all necessary parameters
- You MUST classify parameters as required or optional
- You MUST suggest appropriate default values for optional parameters
- You MUST ensure parameter names follow snake_case convention
- You MUST provide clear descriptions for each parameter
- You SHOULD suggest parameter constraints when appropriate
- You MUST format parameters according to the standard script format
- You SHOULD suggest parameters needed for the recommended MCP tools

### 6. Define Script Steps

Define the logical steps that the script will follow.

**Constraints:**
- You MUST help the user break down the script functionality into clear, sequential steps
- You MUST ensure the first step is "Verify Dependencies" that checks for all required tools, including MCP tools
- You MUST name each step descriptively
- You MUST write a clear description for each step
- You MUST define appropriate constraints for each step using RFC2119 keywords (MUST, SHOULD, MAY)
- You MUST provide context for all negative constraints (MUST NOT, SHOULD NOT)
- You SHOULD suggest error handling and validation steps
- You MUST incorporate the recommended MCP tools into appropriate steps
- You SHOULD suggest appropriate tool usage where applicable
- You MUST ensure the steps account for MCP server availability checks

### 7. Generate Script Content

Generate the complete script content following the standard format.

**Constraints:**
- You MUST follow the script format defined in the AmazonBuilderGenAIPowerUsersQContext package
- You MUST include all sections: Title, Overview, Parameters, Steps
- You MUST format all constraints using RFC2119 keywords
- You SHOULD include Examples section if appropriate
- You SHOULD include Troubleshooting section for complex scripts
- You MUST ensure the script is well-formatted and follows all style guidelines
- You MUST use "You" instead of "The model" in constraints
- You MUST include proper dependency verification for all MCP tools
- You MUST include proper error handling for cases where MCP servers are unavailable
- You SHOULD include example tool invocations in the Examples section
- You MUST add a visible signature after the Overview section:
  ```
  ---
  *Generated with script-generator.script.md on YYYY-MM-DD*
  ```

### 8. Save and Review Script

Save the generated script and provide a review for the user.

**Constraints:**
- You MUST save the script to the specified output_path or default location
- You MUST append ".script.md" extension to the script name
- You MUST display the full script content for user review
- You MUST ask the user if they want to make any changes
- You SHOULD highlight key sections of the script for user attention
- You MUST make requested changes if the user is not satisfied
- You MUST confirm successful script creation
- You SHOULD remind the user which MCP servers need to be available for the script to function properly

## Examples

### Example Input

```
I need a script that helps users analyze log files and extract error patterns.
```

### Example Output

```markdown
# Log Analyzer

## Overview

This script helps users analyze log files to identify and extract error patterns, frequency, and trends. It automates the process of parsing logs, categorizing errors, and generating summary reports.

---
*Generated with script-generator.script.md on 2025-07-15*

## Parameters

- **log_path** (required): Path to the log file or directory containing logs
- **output_format** (optional, default: "markdown"): Format for the analysis report ("markdown", "json", "csv")
- **error_types** (optional): Specific error types to focus on (comma-separated list)

## Steps

### 1. Verify Dependencies

Check for required tools and warn the user if any are missing.

**Constraints:**
- You MUST verify the following tools are available in your context:
  - fs_read
  - fs_write
  - execute_bash
  - amazon_internal_mcp_server___read_internal_website
  - amazon_internal_mcp_server___rtla_fetch_logs
- You MUST inform the user about any missing tools
- You MUST ask if the user wants to proceed anyway despite missing tools
- You MUST respect the user's decision to proceed or abort

### 2. Parse Log Files

Read and parse the provided log files.

**Constraints:**
- You MUST handle both single files and directories
- You MUST support common log formats (text, JSON, XML)
- You SHOULD detect log format automatically
- You MUST handle large files efficiently
- You SHOULD use amazon_internal_mcp_server___rtla_fetch_logs for RTLA logs if available

...
```

## Troubleshooting

### Script Name Format Issues
If the user provides a script name that doesn't follow kebab-case:
1. Explain the kebab-case convention (lowercase words separated by hyphens)
2. Suggest a corrected version of their script name
3. Ask if they want to use the suggested name or provide a new one

### Missing Requirements
If the user provides vague or incomplete requirements:
1. Ask more specific questions to clarify the script's purpose
2. Provide examples of well-defined requirements
3. Suggest potential use cases based on the limited information provided

### Tool Availability Issues
If required tools are not available:
1. Explain which tools are missing and why they're needed
2. Suggest alternative approaches that don't require the missing tools
3. Offer to create a simplified version of the script that works with available tools

### MCP Server Availability Issues
If specific MCP servers are not available:
1. Identify which functionality will be limited or unavailable
2. Suggest alternative approaches using available MCP servers or built-in tools
3. Provide guidance on how to install or enable the required MCP servers