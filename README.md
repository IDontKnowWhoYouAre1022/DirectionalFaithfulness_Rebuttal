# Anonymous Repo for NeurIPS 2026 Rebuttal: Directional Faithfulness in Learned Discretization for Geometric Representation Learning

## Reviewer 4LhS: Qualitative Visualization

**Figure 1.** Direct 5D view of FSQ-style discretization on the fixed Swiss roll tangent pair at $K=512$. Each coordinate panel plots $u_j(t)=[z_j(t) - z_j(base)]/[Q_{0.95}(z_j) - Q_{0.05}(z_j)]$ for one of the five learned latent coordinates and for the pre-specified chart-coordinate directions 0 degrees and 90 degrees. The quantiles are computed once from all saved continuous Swiss roll trajectories, so no tokenizer output determines the coordinate normalization. For each coordinate, symmetric display limits are chosen from the union of the continuous and all three representative paths and then held identical across tokenizer styles. No dimensionality reduction is used. Solid curves are the continuous encoder trajectories; dashed step curves are their code-representative paths. The aligned ribbons show token assignments, with local letter aliases (unique tokens: 2 and 2). For this displayed pair, normalized Hamming disagreement $H=0.16$ (higher is better), full 5D angular error is 59.5 degrees (lower is better), and midpoint-code occupancy $C=0.92$ (lower is better). Directional faithfulness is the evaluation criterion rather than an additional tokenizer. 

<img width="2324" height="2261" alt="swiss_roll_fsq_5d" src="https://github.com/user-attachments/assets/4a72e889-962a-4af5-a1a5-6b4403572a9e" />

**Interpretation:** This is an example of grid-induced local collapse. Both trajectories use only two tokens and are almost entirely assigned to token $A$. The $0^\circ$ trajectory changes token only at its final three samples, while the $90^\circ$ trajectory differs only at its first sample. Consequently, $H=0.16$ and $C=0.92$. More importantly, the two representative vectors differ primarily in $u_1$; the dashed paths remain constant in $u_2,\ldots,u_5$. Thus, the scalar grid registers a large one-coordinate transition while discarding most of the distinct multi-coordinate slopes present in the continuous paths. This produces the large $59.5^\circ$ angular error. The axis-aligned scalar grid is poorly matched to this curved, correlated learned latent geometry. This is consistent with the paper’s argument that structured discretization or nominal utilization does not ensure directional preservation. 


**Figure 2.** Direct 5D view of VQ-style discretization on the fixed Swiss roll tangent pair at $K=512$.

<img width="2324" height="2264" alt="swiss_roll_vq_5d" src="https://github.com/user-attachments/assets/e5306b30-72c5-4398-ad18-876f217f3238" />

**Interpretation:** The VQ-style tokenizer provides the strongest symbolic separation among the three Swiss roll examples. The $0^\circ$ sequence follows approximately $A\rightarrow B\rightarrow C$, whereas the $90^\circ$ sequence follows $B\rightarrow D$. They differ at 18 of the 25 aligned positions, giving $H=0.72$. The dashed paths also reproduce the broad multi-coordinate trends, but replace smooth variation with coarse plateaus and abrupt transitions. The moderate $C=0.60$ shows that both trajectories still spend a substantial portion of their length in the shared midpoint region. The angular error of $22.8^\circ$ indicates that good symbolic separation does not imply the best geometric orientation. VQ distinguishes these two directions well at the token-sequence level, but its piecewise-constant representatives still deform the continuous geometry.


**Figure 3.** Direct 5D view of LGQ-style proxy discretization on the fixed Swiss roll tangent pair at $K=512$.

<img width="2324" height="2261" alt="swiss_roll_lgq_style_5d" src="https://github.com/user-attachments/assets/c81b0abb-3521-479b-98e5-277c9ff0568e" />

**Interpretation:** The LGQ-style proxy preserves the orientation of this displayed pair particularly well. The token patterns are $A\rightarrow B\rightarrow C$ and $D\rightarrow B\rightarrow E$: the two paths use direction-specific tokens at their ends while sharing a central token around the base. This produces $H=0.44$ and $C=0.60$. The dashed paths follow the signs and endpoint trends of the continuous paths across all five coordinates, yielding the smallest displayed angular error, $12.1^\circ$. The comparison with VQ is informative:

- VQ has better symbolic separation: $H=0.72$ versus $0.44$.
- LGQ-style has better displayed angular alignment: $12.1^\circ$ versus $22.8^\circ$.

Adapting the quantization metric can preserve geometric orientation without necessarily maximizing samplewise token disagreement. This demonstrates that $H$ and angular error measure different aspects of directional fidelity. This is a displayed-pair observation, not evidence that the LGQ-style proxy is globally superior. In the aggregate Swiss roll results, VQ actually has a lower average angular error than the proxy.


**Figure 4.** Direct 5D view of FSQ-style discretization on the fixed Torus tangent pair at $K=512$. 

<img width="2324" height="2261" alt="torus_fsq_5d" src="https://github.com/user-attachments/assets/07c5656d-42fa-40cd-9f14-94c9fc385e91" />

**Interpretation:** This is the strongest qualitative failure case. Both trajectories are mapped to the same token $A$ at all 25 positions. Their code-representative paths therefore coincide as one constant 5D vector, even though the continuous trajectories are visibly different. This gives $H=0$, $C=1$, and $\text{angular error}=90^\circ$. The $90^\circ$ value is assigned by convention because a constant representative path has no identifiable principal direction; it should not be described as a literal estimated rotation. Despite a nominal $K=512$ vocabulary, the selected neighborhood has an effective local vocabulary of one token. The tokenizer completely erases the directional distinction. This is the clearest illustration of the paper’s claim that nominal vocabulary size can dramatically overstate effective local directional capacity.


**Figure 5.** Direct 5D view of VQ-style discretization on the fixed Torus tangent pair at $K=512$.

<img width="2324" height="2256" alt="torus_vq_5d" src="https://github.com/user-attachments/assets/b47578c3-fb4c-4f28-8147-bd914519c64b" />

**Interpretation:** The VQ-style tokenizer retains partial directional information, but the two symbolic paths merge quickly. The sequences begin with direction-specific tokens, approximately $A\rightarrow B$ and $C\rightarrow B$, and then share token $B$ over most of the interval. Therefore, only 7 of the 25 aligned positions differ, giving $H=0.28$, while $C=0.78$. The early dashed segments reflect different starting directions in several coordinates, and the angular error remains moderate at $29.4^\circ$. However, the long shared plateau cannot follow the two continuous paths through and beyond the base. Coarse geometric direction remains recoverable, but temporal and symbolic resolution is weak. This panel shows why a reasonable principal direction is not sufficient evidence that the entire trajectory is faithfully discretized.


**Figure 6.** Direct 5D view of LGQ-style proxy discretization on the fixed Torus tangent pair at $K=512$.

<img width="2324" height="2261" alt="torus_lgq_style_5d" src="https://github.com/user-attachments/assets/d492152b-1ecf-4d67-96da-43330796d3aa" />

**Interpretation:** This panel provides the argument for using all three diagnostics together. Both directions remain in token $A$ for 24 of the 25 samples and split only at the final point into $B$ and $C$. Hence $H=0.04$ and $C=0.96$: the symbolic trajectories are almost identical. Nevertheless, the final transitions point in reasonably appropriate 5D directions, producing an angular error of $23.1^\circ$. A principal-axis metric can therefore look favorable even when virtually the entire symbolic trajectory has collapsed. Angular alignment alone can be misleading. A single endpoint transition can recover a coarse direction while providing almost no trajectory-level resolution. $H$ and $C$ expose this failure immediately.

### How They Support the Paper's Main Claims

The figures strongly support the paper's empirical motivation: the same nominal vocabulary size can produce very different local directional representations. They support the usefulness of directional faithfulness diagnostics. 

| Paper Claim | Evidence Supplied by These Figures |
|---|---|
| Pointwise fidelity does not guarantee directional fidelity | In the same run, VQ/LGQ-style have low held-out latent reconstruction errors, approximately $0.006$–$0.010$, yet the torus panels show high midpoint occupancy and weak symbolic separation. | 
| Directional faithfulness is a useful additional evaluation axis | The ribbons and representative paths expose collapse, sequence merging, and angular deformation that nominal $K$ cannot reveal. | 
| Nominal vocabulary size differs from effective local capacity | At $K=512$, each displayed 25-step path activates only 1–3 tokens; torus FSQ activates exactly one. | 
| Tokenizer geometry affects trajectory preservation | The continuous inputs, base point, directions, and scaling are fixed, while VQ-, FSQ-, and LGQ-style discretizations produce markedly different outputs. | 
| Multiple directional metrics are necessary | Swiss VQ versus LGQ-style reverses the ranking under $H$ and angular error; torus LGQ-style combines a reasonable angle with near-total symbolic collapse. | 
| Directional difficulty depends on local geometry | All three torus examples are more midpoint-dominated than their Swiss-roll counterparts, consistent with aggregate Experiment 5. | 







