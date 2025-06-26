#  Level 2 – Major Code Differences

This document explains key differences between the forked repo (`scira-mcp-chat`) and the original (`scira-mcp-chat-1`).

---

##  Change 1: Added OpenAI SDK to `package.json`

Added code-    + "@mcp-ui/client": "^2.2.0",
               + "@mcp-ui/server": "^2.2.0",
               + "ai": "^4.3.16",
Removed code-  - "ai": "^4.3.15",
**Reason:** To use OpenAI GPT models in the frontend.

---

##  Change 2: Created `drizzle.config.ts`
app/api/chat/route.ts-
Added lines-
import { model, type modelID } from '@/ai/providers';
import { smoothStream, streamText, type UIMessage } from 'ai';
// Allow streaming responses up to 60 seconds
export const maxDuration = 60;
More lines are added too

Removed lines-
import { model, type modelID } from "@/ai/providers";
import { smoothStream, streamText, type UIMessage } from "ai";
import { db } from '@/lib/db';
import { chats } from '@/lib/db/schema';
import { eq, and } from 'drizzle-orm';
More lines are removed too

---

##  Change 3: Refactored `useChat.ts` hook
These line are in green that is lines are added-
/**
 * Custom hook for persistent localStorage state with SSR support
 * @param key The localStorage key
 * @param initialValue The initial value if no value exists in localStorage
 * @returns A stateful value and a function to update it
 */
export function useLocalStorageMcpServers<T>(key: string, initialValue: T) {
  // State to store our value
  // Pass initial state function to useState so logic is only executed once
  const [storedValue, setStoredValue] = useState<T>(initialValue);

  const demoServer = {
      id: "MCP-UI-Demo",
      name: "MCP-UI Demo",
      url: "https://remote-mcp-server-authless.idosalomon.workers.dev/sse",
      type: "sse" as const,
      isFixed: true,
      status: "connected" as const,
    };

  // Check if we're in the browser environment
  const isBrowser = typeof window !== 'undefined';

  // Initialize state from localStorage or use initialValue
  useEffect(() => {
    if (!isBrowser) return;

    try {
      const item = window.localStorage.getItem(key);
      if (item) {
        setStoredValue(parseJSON(item));
      }
    } catch (error) {
      console.error(`Error reading localStorage key "${key}":`, error);
    }
  }, [key, isBrowser]);
  More lines are added too.....
**Reason:** Separated message logic from components for reusability.

---

##  Change 5: Removed old unused handlers
app/providers.tsx-
Added lines- 
defaultTheme="ocean"
themes={["light", "dark", "sunset", "black", "ocean"]}

Removed lines-
defaultTheme="system"
themes={["light", "dark", "sunset", "black"]}


**Reason:** To clean up legacy code and reduce confusion.

