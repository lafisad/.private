[[0init]] initializes - as the name suggests (lol)
it checks the system hardware, software, configuration, and signatures, referencing a small internal database for vulnerabilities (such as bypass CVEs etc.). it calculates a secScore, which determines if the system is secure enough to boot.

secScore calculation:
$$
\text{secScore} = 0.3 \cdot S_\text{HW} + 0.4 \cdot S_\text{SW} + 0.2 \cdot S_\text{CFG} + 0.1 \cdot S_\text{SIG}
$$
where:
- $S_\text{HW}$ = hardware security score
- $S_\text{SW}$ = software security score
- $S_\text{CFG}$ = configuration security score
- $S_\text{SIG}$ = signature verification score

does all the checks before boot. if secScore $\ge$ 85%, gives control to [[0boot]]. logs init events for audit. talks to [[0krnl]] for root-of-trust stuff and [[0dev]] for device checks.
