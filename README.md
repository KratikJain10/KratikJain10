<p align="center">
  <img src="./assets/hero.svg" width="100%" alt="Kratik Jain — backend engineer, C++, systems"/>
</p>

<p align="center">
  <a href="https://www.kratix.in"><img src="https://img.shields.io/badge/kratix.in-161b22?style=flat-square&logo=googlechrome&logoColor=58A6FF" alt="kratix.in"/></a>
  <a href="https://www.linkedin.com/in/kratik-jain-015629229/"><img src="https://img.shields.io/badge/LinkedIn-161b22?style=flat-square&logo=linkedin&logoColor=58A6FF" alt="LinkedIn"/></a>
  <a href="mailto:kratikjain1520@gmail.com"><img src="https://img.shields.io/badge/kratikjain1520@gmail.com-161b22?style=flat-square&logo=gmail&logoColor=58A6FF" alt="Email"/></a>
  <a href="https://codeforces.com/profile/kratikjain1520"><img src="https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fcodeforces.com%2Fapi%2Fuser.info%3Fhandles%3Dkratikjain1520&query=%24.result%5B0%5D.rating&label=Codeforces&style=flat-square&logo=codeforces&logoColor=58A6FF&labelColor=161b22&color=1f6feb" alt="Codeforces rating"/></a>
  <a href="https://www.codechef.com/users/kratikjain1520"><img src="https://img.shields.io/badge/CodeChef-1591_·_3★-a371f7?style=flat-square&logo=codechef&logoColor=a371f7&labelColor=161b22" alt="CodeChef 1591, 3 star"/></a>
</p>

<br/>

<a href="https://github.com/KratikJain10/onyx"><img src="./assets/onyx.svg" width="100%" alt="Onyx: Redis-compatible server in C++17. ~1M req/s pipelined, ~68K unpipelined, p99 under 1.7 ms, comparable to Valkey 9.1"/></a>

<details>
<summary><b>The bug the benchmark found: pipelining made Onyx 4× <i>slower</i></b></summary>
<br/>

```text
pipelined, before   ▏ 18K req/s
unpipelined         ███ 68K req/s
pipelined, after    ████████████████████████████████████████████ ~1M req/s
```

Every batch took ~43 ms, whatever its size. The server wrote each reply separately, so **Nagle's algorithm** held back each small write until the previous one was ACKed, while the client **delayed its ACK** waiting for more data. Both sides politely waiting on each other, ~40 ms at a time.

Fix: batch each read's replies into **one `write()`** and set **`TCP_NODELAY`**. **18K → ~1M req/s.** [Full numbers and method →](https://github.com/KratikJain10/onyx#performance)

</details>

<br/>

<p align="center">
  <a href="https://github.com/KratikJain10/RISC-V-Processor"><img src="./assets/riscv.svg" width="49%" alt="RISC-V CPU: RV32IM 5-stage pipeline in Verilog on an FPGA"/></a>
  <a href="https://github.com/KratikJain10/Just-Chat"><img src="./assets/justchat.svg" width="49%" alt="Just-Chat: real-time chat over WebSockets"/></a>
</p>
<p align="center">
  <a href="https://github.com/KratikJain10/GingerartSite"><img src="./assets/gingerart.svg" width="49%" alt="GingerArt: GWoC 2023 production website"/></a>
  <img src="./assets/next.svg" width="49%" alt="Up next: Onyx output buffering, RDB snapshots, more commands; learning Java and Spring Boot"/>
</p>
<!-- VELOX: when it reaches Stage 5 and the repo is public, add assets/velox.svg
     (same style as the cards above) and swap it in for next.svg. -->

<br/>

<p align="center">
  <img src="https://skillicons.dev/icons?i=cpp,c,linux,arch,bash,cmake,git,docker,nodejs,express,mysql,postgres,mongodb,redis,verilog,java&perline=16&theme=dark" alt="C++, C, Linux, Arch, Bash, CMake, Git, Docker, Node.js, Express, MySQL, PostgreSQL, MongoDB, Redis, Verilog, Java"/>
</p>
