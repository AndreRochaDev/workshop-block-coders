# Unlock for Coders — setup check

Hi all,

Thanks for signing up for **Unlock for Coders**, a hands-on workshop on software development with GitHub Copilot.

**When:** 01-10-2026, 9:00–13:00 (4 hours, including two short breaks)
**Where:** Lisbon Room at Republica Office
**Bring:** your laptop and charger

## What we'll do

You'll build a small **.NET 10 Library Loans API and a React web UI** from scratch with GitHub Copilot in VS Code. You'll plan with Copilot, write tests first, review what it generates, open a pull request and get an independent review. No prior AI experience is needed.

Half the session is hands-on, so **please set up and check your laptop before the day**. The check takes about 10 minutes, and it matters: you'll be working on your own machine all afternoon, so a laptop problem found now is a problem that doesn't cost you the workshop.

## 1. Install

| Tool | Notes |
|---|---|
| **VS Code** (latest) | Copilot is built in. Sign in with the GitHub account that has your company's paid Copilot seat (Accounts menu, bottom left). |
| **.NET 10 SDK** | https://dotnet.microsoft.com/download/dotnet/10.0 |
| **EF Core tools** | `dotnet tool install --global dotnet-ef` |
| **Node.js LTS** | https://nodejs.org — needed for the React UI in the afternoon |
| **A Docker-compatible engine** | Docker Desktop, OrbStack (macOS), Rancher Desktop or Podman. Windows: Docker Desktop needs WSL 2. |
| **Git** | Plus a GitHub account that can create a private repository |

On a company laptop, check that you have the admin rights to install these, and that your company allows the Docker engine you pick. Docker Desktop needs a paid licence in larger companies.

## 2. Run the check

Start your Docker engine, open a terminal and run:

```sh
dotnet --version                                  # 10.x
dotnet ef --version                               # prints a version
docker run --rm hello-world                       # prints "Hello from Docker!"
docker pull postgres:17                           # saves time on the day
git --version
node --version                                    # 22.x or newer
npm --version
```

Warm up the npm cache so 20 laptops don't download the same packages at once on the day:

```sh
npm create vite@latest npm-warmup -- --template react-ts
cd npm-warmup && npm install && cd ..
```

Delete the `npm-warmup` folder afterwards.

Then check that NuGet isn't blocked by a proxy:

```sh
dotnet new console -o copilot-precheck
cd copilot-precheck
dotnet add package Npgsql
dotnet run                                        # prints "Hello, World!"
```

Finally, check Copilot:

1. Open the `copilot-precheck` folder in VS Code.
2. Open the Chat view (Ctrl+Alt+I, or ⌃⌘I on macOS) and pick **Agent** in the agents dropdown.
3. Ask: *"Run `dotnet --info` in the terminal and tell me the SDK version."*
4. Approve the command when asked. Copilot should answer with 10.x.

Delete the `copilot-precheck` folder afterwards.

Questions: [ André Rocha, andre.almeida-rocha@axians.com ].

See you on there!
André Rocha
