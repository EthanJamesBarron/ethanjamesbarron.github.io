# Limbs
Limbs are an **integral** thing for creatures in Rain World as they represent arms and legs (if the creature doesn't use tentacles). Think of them like small brains. They reside in **`GraphicsModules`**


The name is pretty much self-explanatory. The idea is you have an array of limbs to start off with. Preferably you'd set this up in the `constructor` of your creature


We're going to be looking at Snails here:

```cs
	limbs = new Limb[2];
	for (int i = 0; i < 2; i++)
	{
		limbs[i] = new Limb(this, snail.bodyChunks[0], i, 0.5f, 0.5f, 0.98f, 5f, 0.5f);
	}
```

Let's go over the parameters:
```cs
new Limb(InstanceOfGraphicsModule, BodyChunkToAttachTo, LimbIndex, LimbRadius, SurfaceFriction, AirFriction, DefaultHuntSpeed, DefaultQuickness);
```