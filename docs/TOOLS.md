<!-- VESPER · the evening watch -->

# The Evening Workshop — Our Tools

The Evening Workshop is the studio where Corey Spangler's drone flights become places
you can step inside. Fly a park, a street, a stretch of Arkansas sky with a
camera drone; bring the footage home; and the tools below rebuild that place
as a Gaussian splat — a cloud of millions of tiny points of light that holds
the real light of the real place — so you can fly it again, walk it, and
build games and scenes on top of it. Everything here is built from open
parts we own end to end: no rented black boxes, no vendor holding the keys.
Two rules run through the whole family. A splat is never shown against a
black void — every surface renders it against a blue sky. And any tool
Corey operates gets a real, interactive surface — a window, a File menu,
buttons — never a program that lives only in a chat window. The mark of the
workshop is a gold feather at the heart of a circular labyrinth, on deep
navy: Daedalus, the craftsman who built wings and flew.

---

## Daedalus

**One-liner:** The whole suite — the name for our end-to-end craft of turning drone footage into explorable 3D places.

Daedalus is the umbrella over everything in this catalog: the maker
(Owlforge), the viewer (Owlsight), and the collision builder (otocoll),
plus the shared conventions they all obey — one shared camera solve per
scene, scenes that grow flight by flight instead of being rebuilt, and
Corey's hands-on verdict as the test that counts. Its splats already power
the flight games built on top of the workshop (On The One, and FlyCabot in
development). Named Nov 24, 2026, for the craftsman who built the labyrinth
and the wings both.

- **Status:** Live — the suite as a whole is in daily use; each tool below carries its own honest state.
- **Needs / pairs with:** A camera drone and somewhere worth flying. Everything else in this catalog is Daedalus at work.

## Owlsight

**One-liner:** The viewer that shows a splat the way it actually is — a true renderer in a single file you can open anywhere.

Owlsight is a single offline HTML file: drag a `.splat` or `.ply` onto it
and you're inside the place, rendered properly — sorted, shaded, and lit
the way the capture really looks, against a blue sky gradient rather than
the black void cheaper viewers leave behind. It exists because the
point-style renderer the website used first was quietly the bottleneck on
every quality judgment; Owlsight is the family's reference eye, the one
place where "is this splat good?" gets an honest answer. It has a real
File menu (Open Splat; Save View as PNG), orbit and WASD/arrow-key flight,
and needs no install, no server, and no internet.

- **Status:** Live — built Nov 24–25, 2026, and in use as the reference viewer. (Corey's formal A/B verdict — the same scene in the old point renderer vs. Owlsight — is still his to give on his own footage; the file he opened first showed the renderer author's demo scene, and we said so plainly at the time.)
- **Needs / pairs with:** Any `.splat` or `.ply` file. Pairs with Owlforge, whose exports are made to be opened in it.

## Owlforge

**One-liner:** The maker — drop in a flight video, get a finished splat out, from a window with buttons instead of a command line.

Owlforge is the workshop's forge: ffmpeg pulls the frames from your flight
video, COLMAP solves where every frame was taken from, and Brush trains
the Gaussian splat and exports it — a pipeline built entirely from open
parts (COLMAP 3.11.1, Brush, ffmpeg) that we own, standing where the
donationware Methodox Splat was the commercial comparison point. You run
it from a real WinForms window (File > Open Video, Make My Splat, a live
log) or by dragging a video onto MakeSplat.bat; a one-time Setup picks a
profile for your graphics card (STANDARD on the RTX 4050 laptop, LITE on
the 4 GB desktop). Version 2 (Nov 25, 2026) added the features that make a
scene a living project: **Add Flights** registers a new flight into a copy
of the scene's existing solve — never a separate solve, and it stops and
warns rather than train garbage if under 70% of the new frames register —
then retrains and exports the next version, old versions untouched;
**auto-tiling** splits big scenes into ~60 m tiles off that one shared
solve (15–20% overlap, measured from the solve, never guessed) for maximum
detail; a **cleanup pass** prunes the giant faint gaussians that draw rings
and haze in true renderers (on the real Richie Road park data it cut
62,760 of 2,392,803 gaussians, 2.6%); and **camera profiles** apply DJI's
official D-Log M to Rec.709 LUT while frames extract, so log footage from
the Avata 360 trains with real color. A PLY-to-.splat converter rides
along in the pipeline.

- **Status:** Staging — v2 is built and staged on both of Corey's PCs (gaius and GOLIATH, 18/18 files, Nov 25, 2026) on top of the proven v1 pipeline that produced the Richie Road park splat. Honest limits from its own README: the Add Flights train step drives Brush headless, so its first run on each machine is a shakedown, and the add-flight acceptance run (Corey's Nov 26 flight into the Richie Road project) has no verdict in the record yet. Sharpest-frame picking is not in yet — frames are taken at a steady rate; that's the first planned refinement.
- **Needs / pairs with:** A Windows PC with a GPU (profiles for 6 GB+ and 4 GB cards), Python on the PATH, and a flight video. Setup downloads its three open tools (~345 MB) once. Pairs with Owlsight (view the exports), otocoll (make them solid), and Daedalus as a whole.

## otocoll

**One-liner:** The tool that gives a splat walls — it builds the invisible collision geometry a game feels but never sees.

A splat is light, not matter: without collision you'd fly straight through
every tree and fence. otocoll is one Python builder that turns a trained
splat `.ply` into a single binary `.otocoll` file — a voxel grid (about
0.75 m cells, opacity/scale-weighted, with floater filtering and a terrain
heightfield) carved by the reconstruction's own camera sight-lines, so
space a camera saw through stays open — plus a small JavaScript runtime
that gives the game swept-sphere collision with a single crash event.
It's deterministic (the same inputs hash to a byte-identical file), and
each collision file is tied by SHA-256 to the exact visual cuts it belongs
to, so a mismatched scene is refused at load instead of failing silently.
Hand-authored overrides (a capsule for a pole, a clear-box for a phantom
wall) merge in at build time, and the builder emits an offline 2D slice
viewer so a scene's collision can be inspected before anyone flies it.

- **Status:** Live — built for the On The One splat work (Splat v2, Part 2B), test suite passing and verified Nov 23, 2026, and in use by the game's splat scenes. An Owlforge add-on that derives a *draft* .otocoll automatically inside the forge exists as a written spec (Nov 25, 2026) but is not built yet — today the builder is run as its own step.
- **Needs / pairs with:** Python 3 with numpy, scipy, and opencv-python; a trained splat `.ply` and its `.splat` cuts. Pairs with Owlforge (which makes the splat) and the flight games that fly it.

---

*Catalog notes, for honesty: Methodox Splat is the commercial comparison
point this workshop measured itself against — it is not ours, and nothing
of it is in these tools. The games (On The One, Drone Hero, FlyCabot) are
creative works that fly what the workshop makes; they are catalogued with
the studio's works, not its tools.*
