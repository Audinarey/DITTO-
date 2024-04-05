# DITTO - Edge case discussion


## Sponsor - warden conversation 
**@audinarey** — Yesterday at 16:34
I noticed while exiting the socialised debt is spread to other currently healthy positions, however, depending on the severity of the amount of debt being socialised and current price action (which can decrease a position's CR), some currently healthy positions can become unhealthy due to an increase in their ercDebt .

Under normal circumstance the health of each position should be checked to ensure it is still healthy after distributing the socialised debt, this is because the burnMsgSenderDebt(...) functiion calls the asset burnfrom(...) function which can be frontrun to liquidate this newly unhealthy position and the user who was innocently closing his healthy position gets nothing in return. (edited)

**@dittoproj** — Yesterday at 16:46
if a SR exists before ercDebt is socialized i believe the shorter should be responsible for the downstream effects?

**@dittoproj**
if a SR exists before ercDebt is socialized i believe the shorter should be responsible for the downstream effects?

**@audinarey** — Yesterday at 16:47
I dont quite understand what you mean here did you mean after debt is socialized? (edited)
[16:50]
Yes shorter should be responsible for the downstream effect, but the fact that it can make the SR's position liquidatable and actually make the Shorter loose all their collateral due to an attacker frontrunnig them to liquidate their position which the socialization has now made vulnerable (edited)

**@audinarey**
I dont quite understand what you mean here did you mean after debt is socialized? (edited)

**@dittoproj** — Yesterday at 16:58
i mean before as in the SR has to exist before (and after) the ercDebt is socialized. If an SR is created after a socialization event happens then they are not resonsible for the increased ercDebt
[17:02]
it sounds like you understand the system but believe it's unfair? the main idea is to protect the peg of the minted assets (ie. dUSD) and so the shorters can be vulnerable to ercDebt increases, but for that risk they are rewarded with yield
[17:02]
and when you say frontrunning are you referring to a specific scenario that happens in the same block?

**@dittoproj**
and when you say frontrunning are you referring to a specific scenario that happens in the same block?

**@audinarey** — Yesterday at 17:09
One scenario is in the same transaction/ block
Another scenario is when the shorter exits half (or a portion) of their position (post socialisation) and the position becomes unhealthy due to socialisation and the remaining position is still unhealthy, unknown to the shorter, in this scenario anyone can liquidate them without frontrunning their transaction
 (edited)
[17:11]
Yes I believe its unfair for shorters who took a position before socialisation happened, but if this is done to shorters who opened a position AFTER socialisation there is no problem because their positions will start out with the new Asset.ercDebtRate (edited)
[17:14]
The fact that the socialised debt is being spread out irrespective of the time the position was opened is not a problem, the problem is positions can become unhealthy due to the socialisation and exit can be done partially

**@dittoproj** — Yesterday at 17:16
hm what is your suggestion? I don't see it being unfair that socialization happens and that it also might make some people under CR, people should know that is possible and increase collateral or exit before this happens

**@audinarey** — Yesterday at 17:17
It doesn't add up if they are rewarded with yield but yet leave some value on the table for liquidators unknown to them,
So they earn yield and loose some of their collateral which may be more than the yield

**@dittoproj** — Yesterday at 17:17
the socialization doesn't happen unless someone is under collateralized and there is nothing in the tapp, someone should have tried to liquidate it earlier etc (edited)

**@dittoproj**
hm what is your suggestion? I don't see it being unfair that socialization happens and that it also might make some people under CR, people should know that is possible and increase collateral or exit before this happens

**@audinarey** — Yesterday at 17:18
During the exit phase after applying the socialised debt, add a check for the health of the position to ensure the position remains healthy even after a partial exit. I will add this to my report (edited)

**@dittoproj**
the socialization doesn't happen unless someone is under collateralized and there is nothing in the tapp, someone should have tried to liquidate it earlier etc (edited)

**@audinarey** — Yesterday at 17:21
Yes correct, but in extreme cases of this scenario, we cannot necessarily tell the value that will be socialized in this scenario, hence instead unknowingly creating unhealthy positions by design (No disrespect to you and your team) we still want apply caution as we may end up socializing more debt and sink the protocol if more position become liquidatable when the TAPP cannot cover the current debt (edited)

**@audinarey**
During the exit phase after applying the socialised debt, add a check for the health of the position to ensure the position remains healthy even after a partial exit. I will add this to my report (edited)

**@dittoproj** — Yesterday at 17:23
what is the check then? does it revert?

**@dittoproj**
what is the check then? does it revert?

**@audinarey** — Yesterday at 17:26
I'll come up with something that tentatively doesnt cause side effects
but right of the top of my head:
we can revert (seems trivial)
we can use a simple algorithm to check that the amount of socialization applied to each position doesn't push it into an unhealthy state. (be careful of side effects here)
 (edited)
[17:32]
I'll like to add our conversation to my report to aid the judges, do you mind?

**@dittoproj** — Yesterday at 17:37
no problem go ahead!

just some final thoughts though for context:
i don't think someone exiting their short will be targeted for frontrunning since the CR actually goes up after exit, so would make sense to target anyone else
ercDebt socialization which leads to lower CRs is just how the protocol is designed to protect the peg

but good work and good questions, it looks like you really understand the protocol, interested to see what algo you come up with!

**@audinarey** — Yesterday at 17:45
Thanks for your kind words

**@dittoproj**
no problem go ahead! just some final thoughts though for context: i don't think someone exiting their short will be targeted for frontrunning since the CR actually goes up after exit, so would make sense to target anyone elseercDebt socialization which leads to lower CRs is just how the protocol is designed to protect the peg but good work and good questions, it looks like you really understand the protocol, interested to see what algo you come up with!

**@audinarey** — Yesterday at 17:46
I'll like to stress that shorters who exit in parts will be hit the most
[17:47]
because since the position is already unhealthy unknown to them, they will loose their remaining position as it will be liquidated (primary liquaidation) of which the yield may not be up to the loss they suffer

Also need I mention that the reputation of the protocol is as important as the PEG (lol) (edited)

**@dittoproj**
no problem go ahead! just some final thoughts though for context: i don't think someone exiting their short will be targeted for frontrunning since the CR actually goes up after exit, so would make sense to target anyone elseercDebt socialization which leads to lower CRs is just how the protocol is designed to protect the peg but good work and good questions, it looks like you really understand the protocol, interested to see what algo you come up with!

**@audinarey** — Yesterday at 17:48
I'll do my best with an algo
[17:49]
I appreciate your patience and your explanations of concepts that strengthened my understanding of the protocol too. (edited)

**@audinarey**
because since the position is already unhealthy unknown to them, they will loose their remaining position as it will be liquidated (primary liquaidation) of which the yield may not be up to the loss they suffer Also need I mention that the reputation of the protocol is as important as the PEG (lol) (edited)

**@dittoproj** — Yesterday at 18:07
haha yes reputation is v important

one more thing, users will be able to see if any SR is in danger of being undercollateralized/liquidated (bc of events) and have knowledge of TAPP levels so to some degree there would be knowledge of an ercDebt socialization about to happen
[18:10]
in fact, bc of liquidation fees the user should initiate the ercDebt event themselves

**@dittoproj**
haha yes reputation is v important one more thing, users will be able to see if any SR is in danger of being undercollateralized/liquidated (bc of events) and have knowledge of TAPP levels so to some degree there would be knowledge of an ercDebt socialization about to happen

**@audinarey** — Yesterday at 18:10
Good to know users will be kept abreast of the socialisation in advance, but there will still be positions in the system.

Partially exited positions that have become undecollatetalised are at the most risk here (edited)

**@dittoproj**
haha yes reputation is v important one more thing, users will be able to see if any SR is in danger of being undercollateralized/liquidated (bc of events) and have knowledge of TAPP levels so to some degree there would be knowledge of an ercDebt socialization about to happen

**@audinarey** — Yesterday at 18:12
but  is there a mechanism in place for users to know how their positions will be affected by the socialisation? (edited)

**@dittoproj**
in fact, bc of liquidation fees the user should initiate the ercDebt event themselves

audinarey — Yesterday at 18:20
What do you mean by this please?

**@audinarey**
but is there a mechanism in place for users to know how their positions will be affected by the socialisation? (edited)

**@dittoproj** — Yesterday at 20:17
it can be calculated with the events

**@audinarey**
What do you mean by this please?

**@dittoproj** — Yesterday at 20:19
if a user ever sees a SR that is liquidatable they should call the method bc they will be paid fees

so in our scenario here im not worried about a user who will be partially exiting their SR bc they would be able to see if a low CR SR is about to be liquidated and the effects it would have on ercDebtRate. And the fact that they see that means that they should just do the liquidation themselves since it's available and they can be paid a fee (edited)

**@dittoproj**
if a user ever sees a SR that is liquidatable they should call the method bc they will be paid fees so in our scenario here im not worried about a user who will be partially exiting their SR bc they would be able to see if a low CR SR is about to be liquidated and the effects it would have on ercDebtRate. And the fact that they see that means that they should just do the liquidation themselves since it's available and they can be paid a fee (edited)

**@audinarey** — Yesterday at 21:04
Given that they can do liquidation themselves puts them (the liquidation) at the risk of frontrunners
[21:05]
And also we do not know the extent to which the event will drive some positions into undercollateralisation but in the event that a position becomes eligible for secondary liquidation, it will no longer be attractive to liquidate for even the shorter

**@audinarey**
And also we do not know the extent to which the event will drive some positions into undercollateralisation but in the event that a position becomes eligible for secondary liquidation, it will no longer be attractive to liquidate for even the shorter

**@audinarey** — Yesterday at 21:06
Edge case thought here
