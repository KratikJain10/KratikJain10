<img src="https://capsule-render.vercel.app/api?type=venom&color=0:0d1117,40:1f6feb,100:a371f7&height=230&section=header&text=Kratik%20Jain&fontSize=60&fontColor=ffffff&fontAlignY=40&desc=from%20transistors%20to%20TCP&descSize=18&descAlignY=62&animation=fadeIn&stroke=58a6ff&strokeWidth=1" width="100%"/>

<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=600&size=20&duration=2600&pause=800&color=58A6FF&center=true&vCenter=true&width=640&lines=wrote+a+Redis-compatible+server+in+C%2B%2B17;~1M+req%2Fs+on+a+single+thread;designed+a+RISC-V+CPU+and+ran+it+on+an+FPGA;built+real-time+chat+on+WebSockets;hunted+a+43+ms+stall+down+to+TCP" alt="typing" />

[![kratix.in](https://img.shields.io/badge/kratix.in-0d1117?style=for-the-badge&logo=googlechrome&logoColor=58A6FF)](https://www.kratix.in)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/kratik-jain-015629229/)
[![Email](https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:kratikjain1520@gmail.com)
<br/>
[![Codeforces](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fcodeforces.com%2Fapi%2Fuser.info%3Fhandles%3Dkratikjain1520&query=%24.result%5B0%5D.rating&label=Codeforces&style=for-the-badge&logo=codeforces&logoColor=white&color=1F8ACB)](https://codeforces.com/profile/kratikjain1520)
[![CodeChef](https://img.shields.io/badge/CodeChef-1591_·_3★-5B4638?style=for-the-badge&logo=codechef&logoColor=white)](https://www.codechef.com/users/kratikjain1520)
![visitors](https://komarev.com/ghpvc/?username=KratikJain10&style=for-the-badge&color=a371f7&label=visitors)

</div>

```console
kratik@arch:~$ whoami
backend engineer · C++ first · NIT Surat '25 (ECE)

kratik@arch:~$ cat interests.txt
sockets · event loops · wire protocols · CPU pipelines · making slow things fast

kratik@arch:~$ ls ~/learning
java/  spring-boot/
```

---

## 🧱 Things I've built

### ⚡ [Onyx](https://github.com/KratikJain10/onyx) · Redis-compatible server in C++17

No libraries, just POSIX sockets. Point `redis-cli` at it and it works. One thread, a `poll()` event loop, an incremental RESP2 parser that handles partial, pipelined and malformed input, and Redis-style lazy + sampled active key expiry.

<table>
<tr>
<td align="center"><h3>~1M</h3><sub>req/s pipelined</sub></td>
<td align="center"><h3>~68K</h3><sub>req/s unpipelined</sub></td>
<td align="center"><h3>&lt;1.7 ms</h3><sub>p99 latency</sub></td>
<td align="center"><h3>0</h3><sub>dependencies</sub></td>
<td align="center"><h3>≈ Valkey 9.1</h3><sub>same machine</sub></td>
</tr>
</table>

```mermaid
flowchart LR
    C["clients"] --> P{{"poll()<br/>one thread"}}
    P -->|readable| R["read → per-client buffer"]
    R --> X["RESP2 parser<br/>complete · partial · malformed"]
    X --> K[("keyspace")]
    K --> W["all replies → one write()"]
    P -->|every 100 ms| E["active expiry<br/>sample 20 keys"]
    E --> K
```

<details>
<summary><b>🐛 The bug the benchmark found: pipelining made it 4× slower</b></summary>
<br/>

```text
pipelined, before   ▏ 18K req/s
unpipelined         ███ 68K req/s
pipelined, after    ████████████████████████████████████████████ ~1M req/s
```

Every batch took ~43 ms, whatever its size. The server wrote each reply separately, so **Nagle's algorithm** held back each small write until the previous one was ACKed, while the client **delayed its ACK** waiting for more data. Both sides politely waiting on each other, ~40 ms at a time.

Fix: batch each read's replies into **one `write()`** and set **`TCP_NODELAY`**. **18K → ~1M req/s.** [Full numbers and method →](https://github.com/KratikJain10/onyx#performance)

</details>

<table>
<tr>
<td width="50%" valign="top">

### 🧠 [RISC-V CPU](https://github.com/KratikJain10/RISC-V-Processor)
**RV32IM, 5-stage pipeline, in Verilog**

Built from scratch and run on a Basys-3 (Artix-7) FPGA. Scoreboard-based issue, operand forwarding, a pipelined multiplier and an iterative divider. Plus a Python/Bash/TCL regression flow that assembles tests and runs them through Vivado automatically.

<img src="https://skillicons.dev/icons?i=verilog,py,bash&theme=dark" height="32"/>

</td>
<td width="50%" valign="top">

### 💬 [Just-Chat](https://github.com/KratikJain10/Just-Chat)
**Real-time chat, MERN + Socket.IO**

Instant messaging over WebSockets, JWT auth with bcrypt-hashed passwords, online presence, conversations stored in MongoDB.

<img src="https://skillicons.dev/icons?i=mongodb,express,react,nodejs&theme=dark" height="32"/>

</td>
</tr>
<tr>
<td width="50%" valign="top">

### 🎨 [GingerArt](https://github.com/KratikJain10/GingerartSite)
**GWoC 2023**

Production website for GingerArt, a startup out of SVNIT Surat.

<img src="https://skillicons.dev/icons?i=html,css,js&theme=dark" height="32"/>

</td>
<td width="50%" valign="top">

### 🔭 Up next

- **Onyx:** output buffering on `POLLOUT`, RDB snapshots, `DEL` / `EXPIRE` / `TTL`
- **Java + Spring Boot:** building something real with it
<!-- VELOX: when it reaches Stage 5 and the repo is public, replace the line above with:
- 🚕 **[Velox](https://github.com/KratikJain10/velox):** ride-hailing backend, Java · Spring Boot. <one line + one measured number>
-->

</td>
</tr>
</table>

---

## 🛠️ Toolbox

<div align="center">

<img src="https://skillicons.dev/icons?i=cpp,c,linux,arch,bash,cmake,git,docker&theme=dark" />
<br/>
<img src="https://skillicons.dev/icons?i=nodejs,express,mysql,postgres,mongodb,redis,verilog,java&theme=dark" />

</div>

---

## 📊 Activity

<div align="center">

<img src="./profile-3d-contrib/profile-night-rainbow.svg" alt="3D contribution graph" width="100%"/>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/KratikJain10/KratikJain10/output/github-snake-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/KratikJain10/KratikJain10/output/github-snake.svg" />
  <img alt="snake eating my contribution graph" src="https://raw.githubusercontent.com/KratikJain10/KratikJain10/output/github-snake-dark.svg" width="100%"/>
</picture>

</div>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:a371f7,60:1f6feb,100:0d1117&height=110&section=footer" width="100%"/>
