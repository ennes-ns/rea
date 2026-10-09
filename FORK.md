# Eigen fork: ennes-ns/rea

Deze clone (`~/Work/repos/rea`) is een **zelf gepatchte en zelf gebouwde** fork van
[morluto/rea](https://github.com/morluto/rea). REA draait hier niet uit npm.

- `origin`: `ennes-ns/rea` (eigen fork, hierheen pushen)
- `upstream`: `morluto/rea` (alleen ophalen; **nooit PR's naar upstream sturen**)

## Bijwerken

```bash
rea-update
```

Dit script (`fork/rea-update`, gesymlinkt als `~/.local/bin/rea-update`) haalt `upstream/main` op, merget het in de eigen `main`,
draait `npm ci` en `npm run build` en voert de tests van de eigen patches uit. Het controleert ook
of de Omarchy-patch nog actief is, en pusht daarna naar `origin`. Herstart daarna Claude Code.

Gebruik **niet** `rea update` of `npx rea-agents setup`: die werken op de npm-installatie of
herschrijven de MCP-registratie naar het npm-pakket.

Bij een merge-conflict stopt het script. Los het conflict op, houd de eigen patches hieronder intact,
commit en draai `rea-update` opnieuw.

## Eigen patches (moeten na elke upstream-merge blijven bestaan)

| Patch                                           | Bestanden                                                                                                                                                                   | Waarom                                                                                                          |
| ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Omarchy als ondersteunde Arch-host (`6fe78015`) | `src/application/LinuxHopper.ts` (`id === "omarchy"`), meldingen in `DoctorDiagnostics.ts` en `SetupHost.ts`, tests, `docs/installation.md`, `docs/user-visible-errors.csv` | Omarchy meldt `ID=omarchy`; upstream accepteert alleen `arch`/`cachyos`, waardoor doctor `unsupported_host` gaf |

Voeg nieuwe patches toe aan deze tabel.

## Hoe het lokaal is aangesloten

- **MCP (Claude Code, user-scope in `~/.claude.json`):**
  `node ~/Work/repos/rea/scripts/rea.mjs mcp` met env
  `GHIDRA_INSTALL_DIR=/opt/ghidra` en `JAVA_HOME=/usr/lib/jvm/java-21-openjdk`.
- **Skill:** `~/.claude/skills/reverse-engineer-anything` → `skills/reverse-engineer-anything`
  (door de build gegenereerd uit `skill-src/`, en daardoor gelijk aan de gebouwde server).
- **CLI:** `~/.local/bin/rea` → `fork/rea`, een wrapper rond `scripts/rea.mjs` met dezelfde env.
- **Ghidra:** pacman-pakket `ghidra` (12.1.x) in `/opt/ghidra`. REA eist een volledige JDK 21+;
  de standaard-Java van het systeem (26) is alleen een JRE, daarom staat `JAVA_HOME` op JDK 21.
