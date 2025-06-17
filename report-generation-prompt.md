# Claude Code ROI Report Generation Prompt

Use this prompt with Claude Code's Linear MCP integration to generate comprehensive productivity reports.

## Prerequisites

Ensure Linear MCP is configured:
```bash
claude mcp add linear -s user -- npx -y mcp-remote https://mcp.linear.app/sse
```

## Report Generation Command

```bash
claude -p "Using the Linear MCP, analyze our team's velocity for the last sprint and combine with these Claude Code metrics:

{
  \"claude_code_sessions\": 42,
  \"total_cost\": 103.45,
  \"pull_requests\": 23,
  \"avg_session_duration\": \"28.5 minutes\",
  \"top_users\": [\"alice@company.com\", \"bob@company.com\"],
  \"cost_per_pr\": 4.50,
  \"token_usage\": {
    \"input_tokens\": 3245670,
    \"output_tokens\": 2156780,
    \"input_cost\": 32.46,
    \"output_cost\": 70.99
  },
  \"tool_usage\": {
    \"Edit\": 356,
    \"MultiEdit\": 128,
    \"Write\": 73,
    \"Read\": 892,
    \"Bash\": 204
  },
  \"tool_acceptance_rates\": {
    \"Edit\": 0.81,
    \"MultiEdit\": 0.92,
    \"Write\": 0.65
  }
}

Generate a comprehensive productivity report that includes:
1. Executive summary with velocity improvements
2. Usage patterns and engagement metrics
3. Linear issue completion metrics (use actual Linear data)
4. Cost analysis with visualizations
5. Actionable insights based on tool usage patterns
6. Productivity comparison (before/after Claude Code)
7. Recommendations for optimization

Use Mermaid diagrams for visualizations. Reference specific Linear ticket IDs where relevant."
```

## Sample Output

The generated report is saved in [sample-report-output.md](sample-report-output.md).

## Customization Tips

- Adjust the metrics JSON to match your actual telemetry data
- Add team-specific context in the prompt
- Request specific time periods (e.g., "for June 1-7, 2025")
- Include additional metrics like error rates or deployment frequency
- Ask for comparisons with previous periods

## Automation

For weekly automated reports, create a script:

```bash
#!/bin/bash
# weekly-report.sh

# Gather metrics from Prometheus
METRICS=$(curl -s "http://localhost:9090/api/v1/query?query=..." | jq '...')

# Generate report
claude -p "Using the Linear MCP, analyze our team's velocity... $METRICS ..."
```