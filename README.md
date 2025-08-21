# caching_mcp_tool_list
---
🚀 MCP Server Tool List Caching in OpenAI Agents SDK
❓ Problem
---
Jab bhi Agent ko kisi MCP (Microservices Communication Protocol) server se tools ki list chahiye hoti hai, toh woh list_tools() function ko call karta hai.
Agar yeh call bar-bar aur zyada hota hai (khaas taur par agar server remote ho), toh:

Latency (deri) barh jaati hai

Performance slow ho jaata hai
---
✅ Solution: Tool List Caching

OpenAI Agents SDK mein ek feature hai jisse aap tools ki list ko cache kar sakte hain.

Cache ka matlab: Tools ki list temporary taur par save ho jaati hai

Agli baar list chahiye ho toh server ko dobara request nahi bhejna padta

List directly cache se serve hoti hai
---
⚙️ Caching Ko Enable Kaise Karein

Jab aap MCP client ko initialize karte hain (MCPServerStreamableHttp), tab sirf ek parameter add karna hota hai:

async with MCPServerStreamableHttp(
    params=mcp_params_cached,
    name="CachedClient",
    cache_tools_list=True
):
    ...


First Call → Tools list server se aayegi aur cache ho jaayegi

Next Calls → List directly cache se serve hogi (no server hit)
---
🔄 Cache Invalidate Karna

Agar aap cache ko refresh ya delete karna chahte hain:

await client.invalidate_tools_cache()
---
🧪 Caching Ko Verify Kaise Karein
---
Server Run Karein

uv run python shared_mcp_server/server.py

---
Caching Script Run Karein

uv run python agent_tool_caching.py

---
Logs Dekhein

Server ke logs mein ListToolsRequest ka count check karein

Agar cache_tools_list=True enable hai →

Multiple list_tools() calls ke bawajood, server par kam requests dikhenge

Matlab caching sahi kaam kar rahi hai
---
🎯 Benefits

MCP Server aur Agent ke beech communication tez aur efficient ho jaata hai

Latency reduce hoti hai

Server load kam hota hai
---
👉 Is tarah aap tool list caching feature ka use karke apne MCP + Agent setup ko fast aur optimized bana sakte hain.
