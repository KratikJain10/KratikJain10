<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:161b22,100:58a6ff&height=190&section=header&text=Kratik%20Jain&fontSize=46&fontColor=ffffff&fontAlignY=34&desc=backend%20%C2%B7%20systems%20%C2%B7%20C%2B%2B&descSize=17&descAlignY=56&animation=fadeIn" width="100%"/>

<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=500&size=20&duration=3000&pause=900&color=58A6FF&center=true&vCenter=true&width=620&lines=I+build+the+layer+under+the+API.;Wrote+a+Redis-compatible+server+in+C%2B%2B17.;~1M+req%2Fs+on+one+thread.+No+libraries.;Found+a+43+ms+stall+hiding+in+TCP." alt="typing" />

<br/>

[![kratix.in](https://img.shields.io/badge/kratix.in-0d1117?style=for-the-badge&logo=googlechrome&logoColor=58A6FF)](https://www.kratix.in)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/kratik-jain-015629229/)
[![Codeforces](https://img.shields.io/badge/Codeforces-1F8ACB?style=for-the-badge&logo=codeforces&logoColor=white)](https://codeforces.com/profile/kratikjain1520)
[![CodeChef](https://img.shields.io/badge/CodeChef-1591_·_3★-5B4638?style=for-the-badge&logo=codechef&logoColor=white)](https://www.codechef.com/users/kratikjain1520)
[![Email](https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:kratikjain1520@gmail.com)

</div>

```cpp
struct Kratik {
    const char* role      = "backend engineer";
    const char* main_lang = "C++";
    const char* also      = "C, Linux, SQL, Node.js/Express";
    const char* learning  = "Java";
    const char* likes     = "sockets, event loops, protocols, finding where the time goes";
    const char* runs      = "Arch, btw";
};
```

---

## ⚡ [Onyx](https://github.com/KratikJain10/onyx) — a Redis-compatible server, from scratch

C++17 on raw POSIX sockets. **Zero dependencies.** Point any Redis client at it and it just works.

```console
$ ./build-release/onyx &
$ valkey-cli -p 6379 SET greeting "hello world" PX 5000
OK
$ valkey-cli -p 6379 GET greeting
"hello world"
```

**How it works**

```mermaid
flowchart LR
    C["clients"] --> P{{"poll()<br/>one thread"}}
    P -->|new conn| A["accept"]
    P -->|readable| R["read → per-client buffer"]
    R --> X["incremental RESP2 parser<br/>complete · partial · malformed"]
    X --> K[("keyspace<br/>unordered_map")]
    K --> W["all replies → one write()"]
    P -->|every 100 ms| E["active expiry<br/>sample 20 keys, 25 ms budget"]
    E --> K
```

- **Single-threaded `poll()` event loop** — replaced my first thread-per-client version; no locks, every command atomic
- **Incremental RESP2 parser** — partial, pipelined and malformed input; binary-safe keys and values
- **Redis-style expiry** — lazy on read + sampled active expiry, on a monotonic clock

**Benchmarks** (`valkey-benchmark`, 1M requests, 50 clients, same machine as Valkey 9.1)

| | Onyx | Valkey 9.1 |
|---|---:|---:|
| `SET`, pipelined `-P 16` | **965K req/s** | 699K req/s |
| `PING`, pipelined `-P 16` | **1.01M req/s** | 1.01M req/s |
| `GET`, one request in flight | **67K req/s** | 68K req/s |

Comparable to Valkey, not "faster than Valkey": Valkey does far more per command. [All numbers + method →](https://github.com/KratikJain10/onyx#performance)

### 🐛 The bug the benchmark found

```text
pipelined, before   ▏ 18K req/s
unpipelined         ███ 68K req/s
pipelined, after    ████████████████████████████████████████████ ~1M req/s
```

Pipelining made it **4× slower**. Every batch took ~43 ms, whatever its size. The server wrote each reply separately, so **Nagle** held back each small write until the last one was ACKed, while the client **delayed its ACK** waiting for more. A polite TCP deadlock, ~40 ms at a time.

Fix: batch each read's replies into **one `write()`** and set **`TCP_NODELAY`**. **18K → ~1M req/s.**

<!-- VELOX: uncomment when it reaches Stage 5 (and the repo is public)
### 🚕 [Velox](https://github.com/KratikJain10/velox) — ride-hailing backend

Java · Spring Boot. <one line on what it does + one real, measured number>
-->

---

## 🛠️ Stack

<div align="center">

<img src="https://skillicons.dev/icons?i=cpp,c,linux,arch,bash,cmake,git&theme=dark" />
<br/>
<img src="https://skillicons.dev/icons?i=nodejs,express,mysql,postgres,mongodb,redis,docker,java&theme=dark" />

</div>

---

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/KratikJain10/KratikJain10/output/github-snake-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/KratikJain10/KratikJain10/output/github-snake.svg" />
  <img alt="snake eating my contribution graph" src="https://raw.githubusercontent.com/KratikJain10/KratikJain10/output/github-snake-dark.svg" width="100%"/>
</picture>

<sub>NIT Surat '25 · ECE · ships C++ that talks TCP</sub>

</div>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:58a6ff,50:161b22,100:0d1117&height=100&section=footer" width="100%"/>
