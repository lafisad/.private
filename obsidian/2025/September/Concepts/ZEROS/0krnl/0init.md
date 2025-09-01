[[0init]] initializes - as the name suggests (lol)
it checks the system hardware and checks a small internal database for vulnerabilities. such as bypass CVEs etc. it calculates a secScore …$$
\text{secScore} = 0.3 \cdot S_\text{HW} + 0.4 \cdot S_\text{SW} + 0.2 \cdot S_\text{CFG} + 0.1 \cdot S_\text{SIG}
$$

$$
\text{boot decision: } \text{if } \text{secScore} \ge 85\%, \text{system boot!}
$$
after that, it gives control to [[0boot]]
