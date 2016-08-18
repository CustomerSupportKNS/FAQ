# SA CONFIGURATION STORAGE FILE
# "A parameter's reference"

[toc]

## INTRODUCTION

### About this document

This document describes how Setup Assistant stores its configuration settings. It can be used as reference material when *analysing* cases that are suspected to be configuration storage related.

- Skill level: Intermediate.
- Required experience: Familiarity with [XML concepts](https://en.wikipedia.org/wiki/XML#Applications_of_XML) (e.g. its syntax).
- Desirable: Affinity with advanced text editing, scripting and ICT in general!

> DISCLAIMER: The information in this document comes with ABSOLUTELY NO GUARANTEES, to the extent permitted by applicable law. The presented information is intended for learning purposes only! It's useage is to be limited to gaining academical / theoretical insights about Setup Assistant's configuration mechanisms! Any form of practical application without supervision of  a - case by case designated - member of the Setup Assistant's development team is prohibited and will have no support from Kulicke and Soffa®!

### Storage file format

Setup Assistant's configuration settings are stored in a "database" storage file: `C:\Program Files\assembleon\SetupAssistant X.xxx.xxx\config\configuration.xml`. It is a plain textfile containing a context sensitive hierachal model of all possible settings using the [eXtensible Markup Language (XML)](https://en.wikipedia.org/wiki/XML#Applications_of_XML) and is therefore *technically* both machine and human readable.

It is designed as a virtually infinitely (!) flexible configuration *storage*. Every thinkable setting combination can be stored in it.  Therefore; albeit *technically* human readable, and build by seemingly simple separate "building blocks" (XML elements and attributes), its interpretation is fairly complex *in practice* requiring elaborate understanding. This is in contrast to using SA's GUI; where having only a general notion of what each system level configuration label means (described in the manual) is enough to be able to understand and configure SA correctly.

### Editing

#### Via the GUI

Setup Assistant (SA) is configured by using the configuration tab from its Graphical User Interface ([GUI](https://en.wikipedia.org/wiki/Graphical_user_interface)). It is a relatively intuitive and safe way for "naive" system users to configure SA's features and make it behave according to the local way of working. Simply checking a checkbox, choosing an option from a list-box, or toggling a radio-button representing a system function, will result in SA storing *all* the corresponding settings correctly in the form of smart *presets* (i.e. configuration setting *combinations*) that are tested to work correctly.

Should configuration changes be required that are not yet present in SA's configuration tab (e.g. from a marketing or customer support perspective) a 'Project Intake Board' request (PIB) or Field Problem Report (FPR) can be filed to have it made available through SA's GUI. Its description should clearly state:

1. The SA function that should be manipulable.
3. The system behavior option(s) that is (or are) missing.
4. The expected default system behavior. 
5. Its rationale from a business case perspective.

When a diagnostic dump is provided along with this information; SA's development team can try to edit the configuration.xml in an effort to better map onto the desired system behavior while awaiting the actual proper software change. This procedure provides short-term workarounds for urgent situational problems that require ad-hoc support (if possible), *as well* as solid / long-term solutions that will benefit multiple (if not all) Kulicke and Soffa® APMR customers!

> Manual ad hoc configuration.xml edits should be considered temporary workarounds and not without risk! E.g. such edits do not persist clean installs (updates should be fine though), nor are they guaranteed to keep working over time (especially when software changes occur).*

#### Directly

Manually editing the storage file in an ad hoc fashion, or 'hacking' if you will, is deemed to be too complex and risky for unguided field application! The rationale for this is (at least) fourfold:

1. Albeit human-readable; the file's syntax is strict and unforgiving: *XML demands a very [specific syntax](https://en.wikipedia.org/wiki/XML). A single character mistake can break the "database structure" of the file, usually rendering the file completely unusable for Setup Assistant.*
2. Configuring a single SA function can be intuitively complex: *Configuring a single function (in the GUI represented by a single setting) generally involves various (context sensitive) settings in the configuration.xml; sometimes not even located closely to each other in the xml structure. Vice versa a single configuration setting might have (an erroneous) impact on various system- and temporal levels that are not always immediately overseeable. Manual edits therefore often yield unintuitive consequences and situations that are difficult to troubleshoot, whereas the presets set via the configuration tab of the GUI are tested to result in a stable system configuration status.*
3. Most (over 95%) of possible configurations are NOT tested: *SA’s system behavior is determined by the combinatorial result of the configuration.xml settings (not by each individually). On top of that its interpretation is context-sensitive, lifting complexity to yet another order of magnitude! It is impossible to manually test all possible configurations (nor to automate it in [polynomial time](https://en.wikipedia.org/wiki/Time_complexity)) because there simply are [too many possible combinations](https://en.wikipedia.org/wiki/Combinatorial_explosion).*
4. To prevent "wild growth" of unknown system configurations: *It is already difficult to keep track of the installed base software versions as it is (requiring a dedicated roll-out group to keep it under control). This would worsen if unguided custom configurations would be allowed, making long-term customer support very difficult, if not impossible.*

In the same vein; ‘a priori’ describing what all is possible using the settings in the configuration.xml can *not* be done either. This may be experienced as counterintuitive, as a *specific* practical situation *can* be dealt with using a *simple* configuration.xml edit (when done correctly). This is because the application logic is irreversible.

> 'Configuration.xml results' are related to its separate 'settings', as to 'stories' are to its separate 'letters': *It is easy to change a story (the result of a configuration.xml) using a limited set of letters (the settings). But it is deemed impossible to describe all stories that can be made by using that very same letter set!*

## STORAGE PARAMETERS

### Jobs

#### Description

Jobs are top-level configuration.xml [elements](https://en.wikipedia.org/wiki/XML#Applications_of_XML) in SA's configuration storage file that represent all actions that SA's users can perform (e.g. the buttons in SA's GUI and/or the corresponding options on a scanner's UI).

#### Example
    <Job presentationSequence="100" class="com.assembleon.saJobs.XxxxxXxxxx" name="XxxxxXxxxx">
      <Lite onTrue="XXXXX" onFalse="XXXXX"/>
      <OnLoadingUnit onTrue="XXXXX" onFalse="XXXXX"/>
    </Job>


#### Attributes

#### RepresentationSequence
A job's configuration.xml element's `RepresentationSequence` *attribute* determines its presentation priority on the various user interfaces. I.e. a job with a higher priority attribute will have its button located higher in the GUI list and/or be presented sooner when cycling through a job-list on the scanners' displays than a job with a lower `RepresentatioPriority` level attributed.

#### Job child elements
Similar to Verifier elements.

### Verifiers
`<Verifier name="XxxxxXxxxx..." class="XxxxxXxxxx...Verifier">`

Verifiers are top-level configuration.xml elements in SA's configuration storage file that represent the requirements that SA checks with each verification round. When actual reality is determined not to meet the specific requirement during its verification; an alarm will be raised. Such alarms can have various severity levels; being: 'ERROR', 'WARNING' or 'OFF'. Which seveirty level should be used for verification is configured (for various contexts) by means of the various specific verifier 'child elements'.

#### Verifier child elements
`<IsXxxxx onTrue="xxxxx" onFalse="xxxxx"/>`

Verifier configuration.xml elements typically have configuration.xml elements embedded of their own (xml being a [hierarchical storage standard](https://en.wikipedia.org/wiki/XML)). Such sub-elements are called 'verifier child elements'. They determine the alarm severity levels to be used by the verifiers containing them, when verification turns out negative in the various contexts.

Typically multiple child elements are present within a verifier. In a way each child element can be regarded a question that is asked by SA to determine the context the verification takes place in. It is their *combination* that corresponds to an actual specific practical context.

Severity levels are effectuated by having filling out the child elements' *attributes* representing the possible various application contexts; `OnLoadingUnit`, `SimulationMode`, etc. enabling differentiation between the various contexts having two fields; `onTrue="xxx"`, or not: `onFalse="xxx"`. The eventually presented alarm severity at verification failure is determined by the the *lowest* level attribute content that is encountered progressing through all child elements during verification ('Off'<'Warning'<'Error').

More concrete examples will be described in the next section of this document.

## PARAMETERS

### Applied configuration.xml edits

#### IsRequired?
`<IsRequired onTrue="xxxxx" onFalse="xxxxx"/>`

The sub-element `IsRequired` enables verification result severity level configuration based on whether the verification target is present on a position in the required setup from the loaded program; `onTrue="xxxxx"`, or not: `onFalse="xxx"`.

*Example use case:* Have SA only check for MaterialID's for loaded feeders *positioned on locations from the required setup of the loaded program* (and raise *errors* in case *those* do not have a MaterialID), but have the verification turned off for the other feeders present in the actual setup (physically on the machine):

    <Verifier name="MaterialMandatoryPropertyPresenceVerifier" class="com.assembleon.saCore.verification.impl.MaterialMandatoryPropertyPresenceVerifier">
      <SetupAssistantEnabled onTrue="ERROR" onFalse="OFF"/>
      <OrderActive onTrue="ERROR" onFalse="ERROR"/>
      <SimulationMode onTrue="OFF" onFalse="ERROR"/>
      <OnLoadingUnit onTrue="ERROR" onFalse="ERROR"/>
      <IsRequired onTrue="ERROR" onFalse="OFF"/>
      <IsEnabled onTrue="ERROR" onFalse="OFF"/>
    </Verifier>

- Subsequently setting its attribute `onFalse="OFF"` would make SA ignore that verifier when its fulfillment is position is *not* in the required setup (IsRequired onFalse); *no* alarm will be raised (OFF).
- Setting its attribute `onFalse="WARNING"`: when a position is *not* in the required setup (IsRequired onFalse); a *warning* will be raised (WARNING), or not (depending on the family setup strictness).
