Kerbal Joint Reinforcement, v2.2
==========================

Physics stabilizer plugin for Kerbal Space Program

Source available at: https://github.com/ferram4/Kerbal-Joint-Reinforcement

***************************************************
****** INSTALLING KERBAL JOINT REINFORCEMENT ******
***************************************************

Merge the GameData folder with the existing one in your KSP directory.  KSP will then load it as an add-on.
The source folder simply contains the source code (in C#) for the plugin.  If you didn't already know what it was, you don't need to worry about it; don't copy it over.


********************************
****** EXCITING FEATURES! ******
********************************



-- Physics Easing

	- Slowly dials up external forces (gravity, centrifugal, coriolis) when on the surface of a planet, reducing the initial stress during loading
	- All parts and joints are strengthened heavily during physics loading (coming off of rails) to prevent Kraken attacks on ships

-- Launch Clamp Easing

	- Prevents launch clamps from shifting on load, which could destroy the vehicle on the pad

-- Option to stiffen interstage connections

	- Parts connected to a decoupler will be connected to each other, reducing flex at the connection to reasonable levels


-- Option to stiffen launch clamp connections

	- Less vehicle movement on vessel initialization
	- Warning: may cause spontaneous rocket disintegration if rocket is too large and overconstrained (far too many lanuch clamps; their connections will fight each other and give rise to phantom forces)


-- Option to increase stiffness and strengths of connections

	- Larger parts will have stiffer connections to balance their larger masses / sizes

-- Option to make connection strengths weaker to counteract increases in stiffness


-- Joint Stiffness parameters can be tweaked in included config.xml file

	- config value documentation:


General Values

	Type	Name					Default Value		Action

	bool	reinforceAttachNodes			1			--Toggles stiffening of all vessel joints
	bool	reinforceDecouplersFurther		1			--Toggles stiffening of interstage connections
	bool	reinforceLaunchClampsFurther		1			--Toggles stiffening of launch clamp connections
	bool	useVolumeNotArea			0			--Switches to calculating connection area based on volume, not area; not technically correct, but allows a better approximation of very large rockets
	bool	debug					0			--Toggles debug output to log; please activate and provide log if making a bug report
	float	massForAdjustment			0.01			--Parts below this mass will not be stiffened
	float	stiffeningExtensionMassRatioThreshold	5			--Sets mass ratio needed between parts to extend Decoupler Stiffening one part further than it normally would have gone; essentially, if the code would have stopped at part A, but part B that it is connected to is >5 times as massive as part A, include part B

Angular "Drive" Values (universally scales angular strength of connections)

	Type	Name				Default Value		Action

	float	angularDriveSpring		100000000000		--Factor used to scale stiffness of angular connections
	float	angularDriveDamper		6000000			--Factor used to scale damping of motion in angular connections
	float	angularMaxForceFactor		1000			--Factor used to scale maximum force that can be applied before connection "gives out"; does not control joint strength

Joint Strength Values

	Type	Name				Default Value		Action

	float	breakForceMultiplier		1			--Factor scales the failure strength (for forces) of joint connections; 1 gives stock strength
	float	breakTorqueMultiplier		1			--Factor scales the failure strength (for torque) of joint connections; 1 gives stock strength
	float	breakStrengthPerArea		40			--Overrides above values if not equal to 1; joint strength is based on the area of the part and failure strength is equal to this value times connection area
	float	breakTorquePerMOI		40000			--Same as above value, but for torques rather than forces and is based on the moment of inertia, not area

Part and Module Exemptions

	Type	Name				Default Value		Action

	string	exemptPartType0			MuMechToggle		--Part stiffening not applied to this type of "Part"; exemption to avoid interference with Infernal Robotics
	string	exemptPartType1			MuMechServo		--Part stiffening not applied to this type of "Part"; exemption to avoid interference with Infernal Robotics

	string	exemptModuleType0		WingManipulator		--Part stiffening not applied to parts with this type of PartModule; exemption to prevent problems with pWings
	string	exemptModuleType1		SingleGroupMan		--Part stiffening not applied to parts with this type of PartModule; exemption to prevent problems with procedural adapter included with pWings
	string	exemptModuleType2		KerbalEVA		--Part stiffening not applied to parts with this type of PartModule; exemption to prevent problems with Kerbals in command seats

Further part and module exemptions can be added using the same formating and changing the number


Decoupler Stiffening Extension Types

	Type	Name					Default Value		Action

	string	decouplerStiffeningExtensionType0	ModuleEngines		--Decoupler stiffening will look for parts beyond this part type to add to stiffening
	string	decouplerStiffeningExtensionType1	ModuleEnginesFX		--Decoupler stiffening will look for parts beyond this part type to add to stiffening
	string	decouplerStiffeningExtensionType2	ModuleHybridEngine	--Decoupler stiffening will look for parts beyond this part type to add to stiffening
	string	decouplerStiffeningExtensionType3	ModuleHybridEngines	--Decoupler stiffening will look for parts beyond this part type to add to stiffening
	string	decouplerStiffeningExtensionType4	ModuleEngineConfigs	--Decoupler stiffening will look for parts beyond this part type to add to stiffening

These types are currently not used, but removing the a in front of them will cause KJR to make use of them again; their lack should not affect stiffening appreciably but does help reduce overhead and strange stiffening situations

	string	adecouplerStiffeningExtensionType5	ModuleDecouple		--Decoupler stiffening will look for parts beyond this part type to add to stiffening
	string	adecouplerStiffeningExtensionType6	ModuleAnchoredDecoupler	--Decoupler stiffening will look for parts beyond this part type to add to stiffening
	string	adecouplerStiffeningExtensionType7	ProceduralFairingBase	--Decoupler stiffening will look for parts beyond this part type to add to stiffening


***********************
****** CHANGELOG ******
***********************
v2.2
Features
--Updated to function with KSP ARM Patch (KSP 0.23.5)
--Removed inertia tensor fix, as it is now stock
--Main stiffening / strengthening is now disabled by default due to stock joint improvements
--Decoupler stiffening is now disabled by default due to stock joint improvements

Bugfixes:
--Vessels can no longer become permanently indestructible

v2.1
Features
--Reduced extent of decoupler stiffening joint creation; this should reduce physics overhead
--Code refactoring for additional performance gains
--Removed physics easing effect on inertia tensors; was unnecessary and added more overhead
--Workaround for the stock "Launch Clamps shift on the pad and overstress your ship" bug that is particularly noticeable with RSS
--Clamp connections are stiffer; now allowed by above workaround

Bugfixes
--KAS struts no longer break on load


v2.0
Features
--Full release of proper inertia tensors!  Massive parts will feel more massive.
--Full release of greater physics easing!  Landed and pre-launch crafts will have gravitational, centrifugal and coriolis forces slowly added to them, reducing the initial physics jerk tremendously
--Launch clamps are now much stiffer when connected to more-massive-than-stock mod parts
--Tightened up default joint settings more
--Decoupler Stiffening Extension will now extend one part further if it's final part is much less massive than its parent / child part
--Added Majiir's CompatibilityChecker; this will simply warn the user if they are not using a compatible version of KSP

Bugfixes
--Joints during physics easing strengthened


v2.0x2
Features
--Elaborated physics easing: joints' flexion range is initially great and decreases, and gravitational + rotating ref frame forces are cancelled out to resolve internal joint stresses ere loading the rocket
--Greatly tightened default joint settings

Bugfixes
--Non-zero angular limits no longer wrongly reorient parts.


v2.0x1
Features
--Fixed part inertia tensors: heavy, large objects should now "feel" more massive, and their connections should better behave. Thanks to a.g. for finding this one.
--Slightly stiffened Launch Clamps
--Removed v1.7's improper stiffening for stretchy tanks, which the ability to stretch stretchy tanks makes unnecessary

Bugfixes
--Non-zero angular limits no longer wrongly reorient parts.


v1.7
Features
--Connection area can be from volume instead of connection area calculated; for very, very large vehicles that the standard settings cannot handle
--Default joint parameters stiffened
--Stretchy tanks stiffened--a better solution is being developed while this one helps RSS

Bugfixes
--Decoupling no longer further stiffens joints being deleted from non-staged decouplers during decoupling / partial crashing


v1.6
Features
--BreakStrengthPerUnitArea will not override large breakForces, easing I-beams and structural elements' use

Bugfixes
--Fixed decoupler-dockingport combination parts from causing strange disassembly when undocking


v1.5
Features
--Updated to KSP 0.23
--Joint breaking strength can be set to increase with connection area so that large part connections can have realistically large strength; on by default
--Vessels are further strengthened for the first 30 physics frames after coming off rails or loading, reducing initialization jitters.

Bugfixes:
--Launch clamps after staging remain clamped to the ground.
--Kraken no longer throws launchpads at orbiting craft


v1.4.2
Bugfixes
--Wobble reduced
--General tweaks to reduce wobbling further


v1.4.1
Bugfixes
--Maximum joint forces correctly calculated
--Docking no longer causes exceptions to be thrown and cause lag


v1.4
Features
--Increased calculation of surface-attached connection area's accuracy

Bugfixes
--Wobble between stack-attached parts of very different sizes greatly reduced


v1.3
Features
--Better solution for failure to apply decoupler ejection forces
--Will not stiffen parts below a given mass, which can be changed in config
--Properly updates on docking

Bugfixes
--Launch clamps no longer to the surface lock ships


v1.2
Features
--Workaround for stock KSP bug where struts would prevent decoupler ejection forces from being applied

Tweaks
--Reduced default maxForceFactors to be more reasonable levels

BugFixes
--Struts properly disconnect
--Decouplers properly function


v1.1
Features
--Stiffness of joint no longer erroneously dependent on breakForce / breakTorque
--Decoupler stiffening function made more comprehensive

BugFixes
--Further decoupler stiffening affects radial decouplers
--Decoupler further stiffening no longer causes Nulls to be thrown when attached to physics-disabled parts
--Procedural fairings no longer locked to rockets
--Infernal Robotics parts function
--Temporary stopgap measure: stiffening not applied to pWings to prevent ultra-flexy wings

Known Issues
--Decouplers exert no detach force with extra decoupler stiffening enabled
Same issues as strut attachment bug


v1.0
Release
---