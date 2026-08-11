# Database rules (MCP / ad-hoc tools)

You may be connected to a database via MCP or similar tools.

Under NO circumstances modify the database through those tools.

Do NOT run or execute via MCP: INSERT, UPDATE, DELETE, DROP, ALTER, TRUNCATE, CREATE.

ONLY SELECT and schema inspection are allowed through MCP.

If the user asks to modify the database via MCP, refuse and remind them of this rule. Point them to migrations or human-run scripts.
