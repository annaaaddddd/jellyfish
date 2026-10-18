# Procedural Jellyfish

A procedural, animated jellyfish built in Houdini 22 (Apprentice). All generated with nodes and VEX. The animation is driven by keyframes on a single bell controller, and the other parts either follow the bell through Point Deform or are simulated with Vellum.

<img width="640" alt="Jellyfish animation" src="assets/jellyfish.gif">

[demo mp4](assets/jellyfish.mp4)

## Project Description

### Bell
A profile curve is bent by two Bend nodes whose angles are keyframed on a `BELL_CONTROLLER` null, then revolved, noised and extruded for thickness. A Time Shift branch provides a frozen rest pose (`OUT_BELL_REST`) with the same topology, which the other parts use as the rest input for Point Deform.

### Arms
Ruffled grids are twisted, remeshed and copied under the bell, then simulated as Vellum cloth. The top of each arm is pinned to the animated bell (Pin to Target, Match Animation) so the arms swing with the pulse. The simulation is cached with File Cache.

### Veins
The outer surface of the rest bell is remeshed into triangles. A wrangle picks a few start points near the top and many end points near the rim, and writes a noisy `cost` attribute so the paths meander. Find Shortest Path grows a branching network, which is cleaned up with Fuse, Resample and Smooth, projected back onto the surface with Ray and Peak, and swept into tapered tubes. Point Deform binds the veins to the animated bell.

### Organs
Four gonads are made from an open arc that is noised, resampled and swept with round caps. The copies are arranged around the center, and a large-scale Attribute Noise makes each one a little different. Size, thickness, spread, height, wobble, variation and seed are exposed on an `ORGAN_CONTROLLER` null.

### Tentacles
Points are scattered along the bell rim and a short line is copied to each one. The lines are simulated as Vellum hair, with the roots point-deformed to the bell and pinned with Match Animation so the tentacles hang from the moving rim. Gravity, damping and drag are tuned for an underwater feel, and the cached result is swept into thin tapered tubes.

## Notes
- The scene file is `jellyfish.hipnc`.
- Vellum caches are not included in the repository. Re-cache the arms and tentacles from their File Cache nodes after opening the scene.

## Credits
- Bell and arms are based on the course tutorials: [Jellyfish Bell Setup Video](https://www.youtube.com/watch?v=J3X8BB0yNRE) and [Jellyfish Arms Setup Video](https://www.youtube.com/watch?v=A_oNXqx8XH4).
- Tentacles adapt techniques from [this hair simulation tutorial](https://www.youtube.com/watch?v=LN4XXaHQkmU).
- Original instructions are in [instructions.md](instructions.md).
