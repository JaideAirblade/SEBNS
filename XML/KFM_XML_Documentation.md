# KFM XML Modding Reference

Quick reference for editing `Jaide_KFM.xml` patches.

---

## XML Structure

```xml
<patches>
    <patch filename="*skill3_contextscriptdata_kungfufighter.xml">
        <select-nodes query="/table/contextscript/stance[@stance='단타']/layer[1]">
            <insert-sibling-before name="layer">
                <append-child name="decision">
                    <append-child name="condition">
                        <append-attribute name="skill" value="CONDITION_ID" />
                    </append-child>
                    <append-child name="result">
                        <append-attribute name="context-2" value="SKILL_ID" />
                        <append-attribute name="control-mode" value="bns" />
                    </append-child>
                </append-child>
            </insert-sibling-before>
        </select-nodes>
    </patch>
</patches>
```

---

## Condition Codes

### Effect/Flag Conditions (300000xxx)
| Code Pattern | Description |
|--------------|-------------|
| `300000XXX` | True if caster HAS effect with flag XXX |
| `310000XXX` | True if caster DOES NOT have effect with flag XXX |
| `320000XXX` | True if target DOES NOT have effect with flag XXX |
| `330000XXX` | True if target HAS effect with flag XXX |

### CSCC Item Conditions (303000xxx / 313000xxx)
| Code | Description |
|------|-------------|
| `303000001` | Searing Dragon Auto - Enables automatic Searing Dragon |
| `303000015` | SB Badge - Auto Bluebuff on SB activation |
| `303000017` | Polarity Badge - Auto Bluebuff on Polarity |
| `313000001` | Searing Dragon Manual - Moves Searing Dragon to LMB |

### Cooldown Conditions (420xxxxxx / 421xxxxxx)
| Code Pattern | Description |
|--------------|-------------|
| `420XXXXXX` | True if skill XXXXXX cooldown group is NOT on cooldown |
| `421XXXXXX` | True if skill XXXXXX cooldown group IS on cooldown |
| `422XXXXXX` | Same as XXXXXX skill condition, ignores cooldown |

### Attribute Conditions (340000xxx)
| Code | Description |
|------|-------------|
| `340000138` | Soul Badge (SB) is active |
| `340000140` | Polarity is active |

### Range Conditions (69001xxx / 69002xxx)
| Code Pattern | Description |
|--------------|-------------|
| `69001XXX` | Target within XX.X meters (e.g., `69001160` = 16.0m) |
| `69002XXX` | Target at or beyond XX.X meters |

---

## Result Slots

| Attribute | Key | Description |
|-----------|-----|-------------|
| `context-1` | LMB | Left Mouse Button |
| `context-2` | RMB | Right Mouse Button |
| `context-3` | F | F key |
| `special-1` | - | Auto-trigger slot (Bluebuff) |
| `special-2` | - | Auto-trigger slot (skills) |
| `skillbar-2` | - | Skillbar slot |

---

## Job Styles

| Value | Element |
|-------|---------|
| `advanced-1` | Fire |
| `advanced-2` | Wind |

---

## Skill IDs

### Fire (advanced-1)
| Skill | ID |
|-------|-----|
| Dragon Claw | 111180-111187 |
| Awakened Dragon Claw | 111700-111706 |
| Burning Fist | 111520-111526 |
| Awakened Burning Fist | 111720-111722 |
| Searing Dragon | 111370 |
| Tremor | 111400 |
| Smolder | 111050 |
| Comet Strike | 111240 |
| Guiding Fist | 111330 |
| Avenging Fist | 111311 |

### Wind (advanced-2)
| Skill | ID |
|-------|-----|
| Cyclone Kick | 112040 |
| Awakened Cyclone Kick | 112720 |
| Roundhouse Kick | 112180 |
| Comet Strike | 112240 |
| Warcry | 112725 |

### Shared
| Skill | ID |
|-------|-----|
| Bluebuff (Fire) | 111000 |
| Bluebuff (Wind) | 112000 |

---

## Common Buff Checks

| Code | Description |
|------|-------------|
| `310000072` | No Liberty Effect |
| `310000100` | Shenlong Form |
| `310000104` | No Raging Volcano |
| `421111400` | Tremor buff active |
| `421111370` | Searing Dragon buff active |

---

## Operations

### Remove nodes
```xml
<select-nodes query="//result[@special-2]">
    <remove />
</select-nodes>
```

### Clear attribute
```xml
<select-nodes query="//result[@context-2='111330']">
    <select-node query="@context-2">
        <set-value value="" />
    </select-node>
</select-nodes>
```

### Add conditional skill
```xml
<insert-sibling-before name="layer">
    <append-child name="decision">
        <append-child name="condition">
            <append-attribute name="field" value="job-style-only" />
            <append-attribute name="job-style" value="advanced-1" />
        </append-child>
        <append-child name="condition">
            <append-attribute name="skill" value="CONDITION" />
        </append-child>
        <append-child name="result">
            <append-attribute name="context-2" value="SKILL_ID" />
            <append-attribute name="control-mode" value="bns" />
        </append-child>
    </append-child>
</insert-sibling-before>
```

---

## Example: Searing Dragon Conditional

```xml
<!-- Manual on LMB with 313000001 -->
<insert-sibling-before name="layer">
    <append-child name="decision">
        <append-child name="condition">
            <append-attribute name="field" value="job-style-only" />
            <append-attribute name="job-style" value="advanced-1" />
        </append-child>
        <append-child name="condition">
            <append-attribute name="skill" value="313000001" />
        </append-child>
        <append-child name="condition">
            <append-attribute name="skill" value="420111370" />
        </append-child>
        <append-child name="result">
            <append-attribute name="context-1" value="111370" />
            <append-attribute name="control-mode" value="bns" />
        </append-child>
    </append-child>
</insert-sibling-before>

<!-- Auto with 303000001 -->
<insert-sibling-before name="layer">
    <append-child name="decision">
        <append-child name="condition">
            <append-attribute name="field" value="job-style-only" />
            <append-attribute name="job-style" value="advanced-1" />
        </append-child>
        <append-child name="condition">
            <append-attribute name="skill" value="303000001" />
        </append-child>
        <append-child name="condition">
            <append-attribute name="skill" value="310000104" />
        </append-child>
        <append-child name="condition">
            <append-attribute name="skill" value="420111370" />
        </append-child>
        <append-child name="result">
            <append-attribute name="special-2" value="111370" />
            <append-attribute name="control-mode" value="bns" />
        </append-child>
    </append-child>
</insert-sibling-before>
```

---

*Reference: [CSCC Item Guide](https://docs.google.com/document/d/1VDFpDSJ_X5vvBee_REPuEWMMs6CKQeCW00GdX6dq2ns/edit)*