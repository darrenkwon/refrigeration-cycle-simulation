# refrigeration-cycle-simulation
Steady-state DWSIM simulation of a propane vapor-compression refrigeration cycle, validated by hand and plotted on a P-h diagram.
How the cycle works

A refrigerator moves heat from a cold space to a warm one, which heat will not
do on its own. The cycle forces it by exploiting the fact that a fluid's boiling
point depends on pressure: propane boils at 253 K (-20 C) at 2.45 bar, but
condenses at 309 K (36 C) at 12.5 bar. Compressing and expanding the same fluid
between those two pressures lets it absorb heat at the low temperature and
reject it at the high one.

**1 -> 2, Compression (+95.31 kJ/kg).** Saturated propane vapor enters the
compressor at 2.45 bar and is compressed to 12.5 bar. Temperature rises from
253 K to 325 K. This is the only work input to the cycle, and the propane leaves
as superheated vapor, well above its 309 K saturation temperature at that
pressure.

**2 -> 3, Condensation (-361.42 kJ/kg).** The hot vapor rejects heat to the
surroundings at constant pressure. It first desuperheats to 309 K, then condenses
to liquid, then subcools a further 3.95 K to the specified 305 K outlet. This is
where the heat absorbed in the evaporator, plus the compressor work, leaves the
system.

**3 -> 4, Expansion (0 kJ/kg).** The liquid passes through a throttling valve
back to 2.45 bar. The valve does no work and transfers no heat, so enthalpy is
unchanged. Temperature nonetheless drops 52 K, because the propane is suddenly
far above its boiling point at the new pressure and 33.7 % of the mass flashes
to vapor. Vaporization requires latent heat, and with no external heat source
that energy comes from the liquid itself, cooling the whole stream.

**4 -> 1, Evaporation (+266.11 kJ/kg).** The cold two-phase mixture absorbs heat
from the refrigerated space at constant pressure and temperature, boiling until
It is saturated vapor. This is the useful output of the cycle: 11.735 kW of
Refrigeration. The stream returns to state 1 and the cycle repeats.

The coefficient of performance is the ratio of what you get to what you pay for:
266.11 kJ/kg of cooling for 95.31 kJ/kg of compression work, or COP = 2.79. On
the P-h diagram these are the horizontal lengths of the 4-1 and 1-2 segments, so
the COP can be read directly off the chart.

Vapor-Compression Refrigeration Cycle (DWSIM)


Steady-state simulation of a propane refrigeration cycle in DWSIM 10.2.5,
Validated through calculations in Excel and plotted on a pressure-enthalpy diagram.

System

| Parameter | Value |
|---|---|
| Refrigerant | Propane (R-290) |
| Property package | Peng-Robinson |
| Evaporating pressure | 2.45 bar (253 K) |
| Condensing pressure | 12.5 bar (305 K) |
| Compressor adiabatic efficiency | 80 % |
| Basis | 1 mol/s (0.0441 kg/s) |

Results

| Quantity | Value |
|---|---|
| Compressor work | 4.203 kW |
| Condenser duty | 15.937 kW |
| Evaporator duty | 11.735 kW |
| COP | 2.79 |
| Carnot COP (253 K / 305 K) | 4.87 |
| Second-law efficiency | 57 % |

Validation
Energy balance closes to 0.0023 %, therefore the 5_evap_out cycles back to 1_evap_out, creating a refrigeration loop within 0.005 K. All three duties were recomputed by Excel from
mass flow x enthalpy change and match DWSIM to five significant figures.

Notes on the model

Evaporator specification. 
The tutorial specifies the evaporator by outlet temperature (253 K). Propane at 2.45 bar is already at 253 K, so this places the PT flash exactly on the saturation dome, where vapor fraction is undetermined; the block returns zero duty. Respecified by outlet vapor fraction = 1.0, which is well posed and physically correct for an evaporator.

Condenser subcooling.
State 3 lies to the left of the saturation curve rather than on it. At 12.5 bar propane saturates at 308.95 K, so the specified 305 K outlet gives 3.95 K of subcooling. This prevents vapor reaching the expansion valve and lowers the flash quality at the evaporator inlet.

Throttling
h3 = h4 = -346.191 kJ/kg exactly; the valve is isenthalpic,
which is why the 3-4 line on the P-h diagram is vertical. 33.7 % of the mass
flashes to vapor across the valve.

Enthalpy reference 
Values are referenced to DWSIM's Peng-Robinson datum and
are negative at these conditions. Only enthalpy differences are physically
meaningful; duties and COP are unaffected by the choice of reference.
