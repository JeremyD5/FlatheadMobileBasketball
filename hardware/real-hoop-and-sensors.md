# Real Hoops, Real Basketballs, and DIY Scoring

Can you skip the Chinese arcade rim, use a regulation hoop with a real net and real basketballs, and put your own sensor in it?

**Yes. And it's easier on a real rim than on an arcade rim.** But it forces three changes to the trailer that are bigger than the hoop decision itself.

---

## The sensor question — answered

Detecting a made shot through an 18" regulation rim is a solved problem with about $15 of parts.

### The workhorse part

**E18-D80NK adjustable infrared photoelectric sensor.** Roughly $8–12. Adjustable detection range from about 3 cm to 80 cm, NPN output, screws straight into a microcontroller GPIO. An 18" rim is 46 cm across, well inside its range. This is the part used in essentially every serious DIY smart-hoop build, including the [Home Assistant smart backboard project](https://www.vcloudinfo.com/2020/09/home-assistant-smart-diy-basketball-backboard-project.html).

An 18" regulation rim gives you **3 more inches of working distance than a 15" arcade rim**, so the sensor problem gets easier, not harder, when you go to a real hoop.

### Use a pair, not one

[US Patent 6418179](https://patents.google.com/patent/US6418179) describes the standard commercial approach: **a pair of photoelectric sensors mounted below the inner rim.** Two sensors rather than one solves two failure modes at once — net interference triggering false positives, and direction discrimination so a ball pushed up through the hoop from underneath doesn't score. The patent also notes why the older mechanical microswitch approach was abandoned: it miscounts on reverse throws and rebounds, and the switch fatigues and wears out.

For a rental unit running 45 events a season with kids gaming it, direction discrimination is not optional. Someone will stuff the ball up through the net.

### Add a vibration sensor

**SW-420 vibration switch**, ~$2. Mounted to the backboard, it distinguishes a swish from a bank shot from a rim-out. The Home Assistant build uses exactly this pairing: IR for "did it go through," vibration for "how did it get there." Opens up scoring variants (swish worth more) which is a genuine differentiator.

### Parts list, per hoop

| Part | Purpose | Cost |
|---|---|---|
| E18-D80NK IR photoelectric sensor ×2 | Shot detection + direction | $16–24 |
| SW-420 vibration sensor | Swish vs bank vs rim-out | $2 |
| ESP32 or ESP8266 D1 Mini | Logic, timer, WiFi | $6–12 |
| XL4016 buck converter | 12V → 5V | $6 |
| LED matrix or 7-segment display | Scoreboard | $25–45 |
| Amp + speaker | Buzzer, announcer | $15–30 |
| Wiring, enclosure | | $15 |
| **Per hoop** | | **$85–134** |

WiFi on the ESP32 means a shared leaderboard across all lanes with no extra hardware. That's something none of the Chinese machines do and it's a real marketing hook.

### Off-the-shelf alternatives

| Option | Notes |
|---|---|
| [Pop-A-Shot scoring sensors](https://popashot.net/products/indoor-outdoor-dual-shot-scoring-sensors) | Sold separately, one pair per hoop. Their infrared system claims **98% shot registration**. Catch: explicitly only compatible with their own games, so you're buying sensors you'd have to reverse-engineer |
| [Swish Hoop Shot Monitor](https://www.swishhoop.com/products/swish-hoop%C2%AE-basketball-skills-trainer-lite) | Wireless, clips to any net with a telescoping pole, claims **99.9% accuracy on a standard indoor hoop**, no batteries to charge. Consumer training product, app-based — probably not the right integration for a rental unit, but proves the accuracy is achievable |

### Rim and net sourcing

Buy the rim separately from any sporting goods or gym equipment supplier. Breakaway rims run roughly $30–150 depending on whether you want a competition-grade spring rim. Nets are $5–20. Neither is exotic.

Reference DIY builds beyond the ones above: [Arduino ScoreKeeper](https://www.instructables.com/Arduino-Home-Basketball-Hoop-Score-Detection-Syste/), [Pop-a-Shot Upgrayedd](https://www.instructables.com/Arduino-Basketball-Pop-a-Shot-Upgrayedd/).

---

## ⚠️ The three things this breaks

The hoop is the easy part. Here is what real basketballs actually cost you.

### 1. Rim height — the big one

Player stands on the ground outside; rim is inside the trailer. So effective rim height = deck height + rim height above the deck.

| | 7 ft interior (standard) | 8 ft interior |
|---|---|---|
| Trailer deck off ground | ~22" | ~22" |
| Interior height | 84" | 96" |
| Backboard needed above rim | ~18" | ~18" |
| Max rim above deck | 66" | 78" |
| **Effective rim height** | **7'4"** | **8'4"** |

Regulation is 10 ft. Youth standard is 8 ft.

**At 7'4" every adult at the party can dunk.** With a 22 oz ball and a steel rim bolted to a trailer wall, that's a structural problem and a liability problem, and it will happen at every single event.

**Action: change the trailer spec to 8 ft interior height.** This was already flagged in STARTUP-COSTS.md as "7 ft minimum, ideally 7'6"." With real hoops it becomes 8 ft, non-negotiable. Narrows the used-trailer search considerably — factor that into the Marketplace hunt.

### 2. Lane count drops from 4 to 3

An arcade station is 41" wide. A real hoop with an 18" rim and a size 7 ball needs more like 48–54".

| Config | Lane width | Total | Fits 16 ft (192")? |
|---|---|---|---|
| 4 arcade lanes | 41" | 164" | Yes, 28" spare |
| 4 real-hoop lanes | 48" | 192" | Exactly zero margin |
| **3 real-hoop lanes** | **52"** | **156"** | **Yes, 36" spare** |
| 4 real-hoop lanes | 48" | 192" | Needs a 20 ft trailer |

**Three lanes in a 16 ft trailer, or four lanes in a 20 ft trailer.** Three lanes means less throughput at festivals, which is where your best margins are. Worth thinking hard about.

This also changes every count in both divider BOMs — 3 lanes needs 4 panels, not 5.

### 3. Ball return and cage both get beefier

- **Capacity drops.** An arcade lane holds 10–12 mini balls. A 9.55" ball takes roughly 3× the volume, so you fit 4–5 per lane. Probably fine for gameplay, but it changes the ramp design.
- **Momentum.** A 22 oz ball coming down a ramp has real energy. The plywood return needs to be wider, better damped, and stopped properly at the bottom.
- **The cage spec goes up.** A 22 oz ball at speed will punch through lightweight netting and will work-harden 2"×1.25" mesh over a season. If you go real balls, the divider mesh and the overhead containment both need to be rated for it.

---

## Cost comparison

| | Arcade (mini balls, 15" rim) | Real (size 7, 18" rim) |
|---|---|---|
| Rim + net, per lane | $35–100 | $35–170 |
| Sensor + electronics, per lane | $85–134 | $85–134 |
| Balls | 25 custom mini @ $600–900 | 20–25 size 7 @ $500–1,000 |
| Lanes that fit in 16 ft | 4 | 3 |
| Trailer height required | 7 ft | 8 ft |

**The hardware costs about the same.** What real basketballs cost you is one lane and a narrower trailer search.

---

## The honest tradeoff

**For:** it feels like real basketball instead of a carnival game. Adults engage instead of watching. Better for corporate events, better for the Range Riders partnership, better for anything with teenagers. Harder for a competitor to copy with an $800 Alibaba machine.

**Against:** you lose a lane, which hurts throughput exactly at the festival bookings that carry the season. The trailer gets harder to find and costs more. And the arcade format has a real advantage you'd be giving up — the mini ball and the moving hoop are what make a 6-year-old and a 40-year-old competitive with each other, which is the whole social dynamic at a birthday party.

**A middle path worth considering:** size 5 youth balls (27.5") with a 16–17" rim. Keeps the "real basketball" feel and a real net, keeps 4 lanes, keeps the rim height workable in a 7'6" trailer, and a 14 oz ball is much easier on the structure than 22 oz.

---

## Open questions

- [ ] Decide ball size before anything else — it drives trailer height, lane count, and both divider BOMs
- [ ] Price size 5 / 16" rim as the middle option
- [ ] Order one E18-D80NK and one rim, mock up a single lane, test detection accuracy with kids
- [ ] Test whether direction discrimination actually works with two sensors before committing to four lanes of it
- [ ] Source royalty-free announcer audio (no NBA or licensed clips)
- [ ] Confirm 8 ft interior trailers are findable used in the Flathead — if not, this whole direction may be off the table
