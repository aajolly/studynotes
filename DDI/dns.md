# DNS, DHCP & IPAM (DDI)
## DNS
![dns_components](/images/ddi/DNS_Components.png)

### DNS Components Explained via a Library Analogy
#### Objective
Resolve the IPv6 address of `eat.example.com` – like finding the shelf number for a book in a vast network of libraries.

**Stub Resolver or Client = You (the reader)**
- You want to read a book called `eat.example.com`.
- But you don’t know where it’s located — so you go ask the front desk librarian.

**Recursive Name Server = Front Desk Librarian**
- This librarian takes your request and promises to find the book for you, no matter how many libraries they must visit.
- They perform all the work on your behalf — this is a recursive query.

**Authoritative Name Servers = The Actual Libraries with Official Records**
- Each library knows about a specific section (zone).
- They answer with authoritative answers like “Yes, I know the shelf location of this book.”

**Zones = Sections of the Library System**
- Each authoritative name server manages a zone — like Fiction, Science, History.
- The zone is a chunk of DNS namespace managed by that server.

**Iterative Query = Librarian Asking Other Libraries**
- If the front desk doesn’t know the answer, they ask other libraries one-by-one:
  - "Do you know where 'example.com' is?"
  - "Can you tell me about 'eat.example.com'?"
- Each library may redirect to another with better knowledge — this is iterative querying.

**Flow Summary:**
1. You (client) ask the librarian (recursive resolver):
    “Where is ‘eat.example.com’?”
2. The librarian searches library-to-library (iterative queries) until they find the right one.
3. Once the authoritative name server (correct library) responds with the exact location (IPv6 address), the librarian gives it to you.