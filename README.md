# Friendship Paradox Explorer

An interactive, browser-based visualizer for the classical and majority-type friendship paradox inequalities in complex networks, based on the following papers:

> **S. H. Lee**, "Friendship-paradox paradox: do most people's friends really have more friends than they do?"
> *Journal of the Korean Physical Society* **88**, 890–897 (2026).
> DOI: [10.1007/s40042-025-01559-4](https://doi.org/10.1007/s40042-025-01559-4)

> **S. H. Lee**, "Two variants of the friendship paradox: The condition for inequality between them."
> *New Physics: Sae Mulli* **76**(2), 169–175 (2026).
> DOI: [10.3938/NPSM.76.169](https://doi.org/10.3938/NPSM.76.169)

---

## What It Shows

The friendship paradox (FP) is usually summarized as *"your friends have more friends than you do."*
But this colloquial phrasing hides several distinct mathematical claims.
The explorer separates them into four quantities computed live on any network:

| Quantity | Definition | What it asks |
|---|---|---|
| ⟨k_friend⟩ | ⟨k²⟩/⟨k⟩ | Alter-based FP: average degree along a random edge |
| ⟨k_nn⟩ | average of k_nn(i) over nodes | Ego-based FP: average of each node's mean-neighbor degree |
| φ_global | fraction { k_i < k_nn(i) } | *Is it likely* that your friends have more friends (mean-based)? |
| φ_local | fraction { h_i < ½ } | *Is it likely* that **most** of your friends have more friends (median-based)? |

The first two are network-level averages and are always ≥ ⟨k⟩ (the classical FP).
The latter two are majority-type quantities — they can independently fall above or below ½ regardless of whether the classical FP holds, and they can diverge from each other when neighbor-degree distributions are skewed.

Node colors encode each node's domination status:

| Color | Meaning |
|---|---|
| 🟢 Green | Not dominated (neither mean nor median) |
| 🔵 Blue | Mean-dominated only — k_i < k_nn(i) |
| 🟠 Amber | Median-dominated only — h_i < ½ |
| 🟣 Purple | Both mean- and median-dominated |

---

## Networks Included

### Toy network (5 nodes)
A minimal hand-constructed example from the paper demonstrating that φ_global = φ_local = 2/5 < ½ is achievable even while both classical FP inequalities hold.

### Zachary's Karate Club (34 nodes, 78 edges)
A benchmark social network where mean-based and median-based domination broadly agree (both φ values ≈ 0.85).

> W. W. Zachary, "An information flow model for conflict and fission in small groups,"
> *Journal of Anthropological Research* **33**(4), 452–473 (1977).
> DOI: [10.1086/jar.33.4.3629752](https://doi.org/10.1086/jar.33.4.3629752)

### American Football (115 nodes, 613 edges)
The Girvan-Newman Division IA college football network (Fall 2000 regular season), where mean-based and median-based notions of domination strongly diverge: φ_global ≈ 0.43 < ½ but φ_local ≈ 0.84.
Node fills encode the 12 conferences; node rings encode the domination category.

> M. Girvan & M. E. J. Newman, "Community structure in social and biological networks,"
> *Proc. Natl. Acad. Sci. USA* **99**, 7821–7826 (2002).
> DOI: [10.1073/pnas.122653799](https://doi.org/10.1073/pnas.122653799)

### Custom
Click anywhere on the canvas to add nodes; click two nodes in sequence to add or remove an edge between them. Drag nodes to rearrange. Use the Star and Ring presets as starting points. All four quantities update in real time.

---

## Usage

The entire app is a single self-contained HTML file with no external dependencies (all JavaScript and CSS are inlined; web fonts load from Google Fonts).

```bash
# Clone and open — that's it
git clone https://github.com/<your-username>/friendship-paradox-explorer.git
open friendship-paradox-explorer.html   # macOS
xdg-open friendship-paradox-explorer.html  # Linux
```

Or just open `friendship-paradox-explorer.html` directly in any modern browser — no server required.

---

## Layout Algorithm

The three built-in networks use a **Fruchterman-Reingold spring layout**:

- Attractive force between connected nodes: f_a(d) = d²/k
- Repulsive force between all node pairs: f_r(d) = k²/d
- Optimal spring length: k = C · √(area / N)
- Temperature-based cooling converges the layout; the graph is pre-warmed synchronously before first paint so it appears already settled

The custom canvas uses a **static layout**: nodes stay exactly where you place or drag them; no spring runs.

---

## Implementation Notes

- Pure HTML + CSS + JavaScript; no frameworks, no build step
- Force layout and metric computation written from scratch (~150 lines each)
- SVG rendering with per-node tooltips showing k_i, k_nn(i), h_i, and domination status
- All six paper-reported statistics for the football network reproduce exactly (verified against Table 1 of the JKPS paper)

---

## Citation

If you use this tool in your work, please cite the two papers listed at the top of this README.

---

## Credits

Network data for the football network: [Mark Newman's network data page](http://www-personal.umich.edu/~mejn/netdata/).

*Created by [Claude Sonnet 4.6](https://www.anthropic.com) · Anthropic.*
