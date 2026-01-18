# 🧪 Lab 06 (2 Hours) — Interactable 2: Targets Hit Score (Ray Select)

Includes scripts + event wiring

---

---

## Objective

Implement an interactive target practice system in VR using ray-based interaction, where users can hit targets to increase a score counter displayed on the HUD, and observe automatic target reset behavior after a delay.

This lab extends the existing GameManager and HUD system to support gameplay scoring and event-driven interactions using the XR Interaction Toolkit.

---

## Learning Outcomes

By the end of this lab, students will be able to:

- Create interactive targets using XR Simple Interactable  
- Handle ray-based selection events in VR  
- Connect XR interaction events to custom C# methods  
- Implement a scoring system using a centralized GameManager  
- Update HUD elements dynamically based on gameplay events  
- Use coroutines to implement delayed object reset behavior  
- Design simple interactive VR gameplay mechanics  
- Extend existing systems across multiple labs in a continuous project  

---

## A) Add Targets UI Text

HUD_Canvas → add TMP text  

Rename: TXT_Targets  

Text: “Targets: 0”

---

## B) Update GameManager.cs

Add these inside GameManager:

Add variables:

```csharp
public int targetsHit;
public TMP_Text targetsText;
```
Add method:
```csharp
public void AddTargetHit() { targetsHit++; RefreshUI(); }
```
Update RefreshUI():
```csharp
if (targetsText) targetsText.text = $"Targets: {targetsHit}";
```
In Inspector: drag TXT_Targets into Targets Text

---

## C) Create Target Prefab

3D Object → Cylinder  

Rename: Target  

Add collider (already has)  

Add Component → XR Simple Interactable  

Drag into Prefabs folder  

---

## D) Script: TargetHit.cs

Scripts → Create TargetHit  

Paste:

```csharp
using System.Collections;
using UnityEngine;

public class TargetHit : MonoBehaviour
{
    public Renderer rend;
    public float resetDelay = 5f;

    private Color original;
    private bool hit;

    void Start()
    {
        if (!rend) rend = GetComponentInChildren<Renderer>();
        if (rend) original = rend.material.color;
    }

    public void Hit()
    {
        if (hit) return;
        hit = true;

        if (rend) rend.material.color = Color.yellow;
        if (GameManager.Instance) GameManager.Instance.AddTargetHit();

        StartCoroutine(ResetTarget());
    }

    IEnumerator ResetTarget()
    {
        yield return new WaitForSeconds(resetDelay);
        if (rend) rend.material.color = original;
        hit = false;
    }
}
```
Attach TargetHit to Target prefab

---

## E) Connect XR Event to Script

Open Target prefab  

Select object with XR Simple Interactable  

In Inspector find Select Entered event  

Click +  

Drag same Target object into event slot  

Dropdown → TargetHit → Hit()

---

## F) Place 8 Targets

Drag prefab into scene 8 times → make a target range.

---

---

## Submission Task

Hit 3 targets and show score increases + reset works.

---

---

⚠️ **Important Note**

Students must submit **all project files to GitHub** along with the demo video.

Both of the following are mandatory:

- Complete Unity project pushed to a GitHub repository  
- Demo video file uploaded or shared with the submission  

Submissions without the GitHub project files will be considered incomplete.

---
---

## Next Lab

Lab 07: Lore object + popup UI.

---



