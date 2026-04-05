# Kitsune Vehicle Overhaul

![Kitsune Vehicle Overhaul](kitsune-overhaul.jpg)

**Your 4x4 truck weighs two tons. A cactus should not total it.**

Kitsune Vehicle Overhaul rebalances vehicle collision damage so driving through the desert doesn't feel like driving through a minefield. Cacti shatter on impact, heavier vehicles shrug off rough terrain, and you stop burning through repair kits every five minutes.

Works with **vanilla vehicles** and **Bdub's Vehicles** out of the box. Bdub's is supported but not required — the mod works fine with just vanilla. Server-side only — no client install needed.

---

## What It Does

### The Cactus Fix

The big saguaro cacti (the tall ones) were missing a `VehicleHitScale` property that the small cacti already had. This meant a 20-foot cactus hit your truck like a concrete wall. We fixed that.

- **Small cacti** shatter instantly with zero damage (vanilla behavior, unchanged)
- **Large saguaros** now shatter easily and leave a tiny scratch at most — enough to hear the "thunk," not enough to care about

### Vehicle Damage Resistance

Every vehicle now has built-in collision resistance based on its weight class. This stacks with the **Intellect Mastery** perk, which already reduces vehicle self-damage by 20% per rank.

| Class | Vehicles | Damage Resistance |
|-------|----------|-------------------|
| Bicycle | Bicycle | -15% |
| Light | Minibike, Golf Cart, Duster, Buggy | -25% to -30% |
| Motorcycle | Motorcycle, Cruiser, Dirt Bike, Junker, Rat Bike | -35% |
| Car | Charger, GNX, Hot Rods, Nova, Stallion, Pickup, Willy Jeep | -40% |
| Truck | 4x4, Box Trucks, Semi, Trophy Truck, Work Truck, SHERP, UAZ | -50% |
| Military | BRDM-2, Humvee, LMTV, Marauder, MRAP | -60% |
| Aircraft | Gyrocopter, MD-500, UH-60 Black Hawk | -25% |

**What this means in practice:** A vanilla 4x4 with zero perks takes full collision damage. With this mod, that same truck takes half damage before you invest a single skill point. Add a few ranks of Intellect Mastery and you're practically bulletproof.

### HP Buffs

All vehicles get a modest ~25% HP increase as a secondary buffer. This is intentionally small — the damage resistance does the heavy lifting, and we didn't want to inflate repair kit costs.

### Repairing

Repair kits work the same as vanilla. Each kit restores a flat 1,000 HP plus a percentage bonus from Intellect Mastery. Because the HP buffs are modest, you'll need at most 1-2 extra kits compared to vanilla on a full repair from zero — and since you're taking less damage in the first place, you'll repair less often overall.

---

## Supported Vehicles

**40 vehicles total** across vanilla and Bdub's Vehicles:

- **Vanilla (5):** Bicycle, Minibike, Motorcycle, 4x4 Truck, Gyrocopter
- **Bdub's Old Variants (5):** Old Bicycle, Old Minibike, Old Motorcycle, Old 4x4, Old Gyrocopter
- **Bdub's Motorcycles (4):** Cruiser, Dirt Bike, Junker, Rat Bike
- **Bdub's Light (3):** Golf Cart, Duster, Buggy
- **Bdub's Cars (10):** Charger, GNX, Hot Rod 1-6, Nova, Stallion, Pickup, Willy Jeep
- **Bdub's Trucks (7):** Box Truck (Plain), Box Truck (Hostess), Old Semi, Trophy Truck, Work Truck, SHERP, UAZ-452
- **Bdub's Military (7):** BRDM-2, BRDM-2 Desert, BRDM-2 New, Humvee, LMTV, Marauder, MRAP
- **Bdub's Aircraft (2):** MD-500, UH-60 Black Hawk

Vehicles from other mod packs are unaffected — this mod won't interfere with them, but they won't get the buffs either.

---

## Installation

Drop the `Kitsune Vehicle Overhaul` folder into your game's `Mods/` directory:

```
7 Days To Die/
  Mods/
    Kitsune Vehicle Overhaul/
      ModInfo.xml
      Config/
        items.xml
        blocks.xml
```

**Server-side only.** Clients don't need to install anything — configs are sent on connect.

---

## Compatibility

- **7 Days to Die V1.0+**
- **Bdub's Vehicles** — full support
- **Vehicle Armor Mod / Vehicle Plow Mod** — stacks cleanly (those use `VehicleStrongSelfDamage`, we use `VehicleSelfDamage`)
- **Grease Monkey / Intellect Mastery perks** — stacks additively as intended
- **Other vehicle mods** — no conflicts, but unpatched vehicles won't receive buffs

---

## Technical Details

For modders who want to understand or tweak the values:

**Collision self-damage formula (from `EntityVehicle.OnCollisionForward`):**
```
raw_damage = max(0, speed - 1.5) * mass * 0.0583
self_damage = min(raw_damage, damage_to_block) * (2.5 / VehicleHitScale) * VehicleSelfDamage
```

**VehicleHitScale** (blocks.xml) — Per-block property. Higher value = block breaks easier AND less self-damage to vehicle. `999` = zero damage, `50` = tiny scratch, `1` = full damage (default).

**VehicleSelfDamage** (items.xml) — Passive effect on vehicle items. Multiplier on all collision self-damage. Stacks additively with Intellect Mastery (-20% per rank) and Fireman's Almanac (-25%).

**Repair formula (from `Vehicle.RepairParts`):**
```
hp_per_kit = 1000 + (max_health * intellect_mastery_rank * 0.1)
```

---

## Credits

- **Kitsune** — Mod design and balance
- **Bdub** — Bdub's Vehicles (the vehicle pack this mod patches)
- **The Fun Pimps** — 7 Days to Die
