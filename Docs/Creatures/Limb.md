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

Now that we understand the parameters and how to create limbs, let's have a look at how you can use them:

```cs
for (int i = 0; i < 2; i++)
	{
		float scaled = 14f * snail.size;
		if (snail.suckPoint != null) // Suckpoint is the middle of the tile the snail is currently on
		{
			limbs[i].mode = Limb.Mode.HuntAbsolutePosition;
			if (snail.AI.move)
			{
				if (i == movingHand)
				{
					Vector2 vector = Custom.DirVec(snail.mainBodyChunk.lastLastPos, snail.suckPoint.Value);
					vector = (vector + (Custom.DirVec(snail.bodyChunks[1].pos, snail.bodyChunks[0].pos) + Custom.PerpendicularVector((snail.bodyChunks[1].pos - snail.bodyChunks[0].pos).normalized) * 0.5f * ((i == 0) ? (-1f) : 1f))).normalized * scaled * 0.9f + snail.suckPoint.Value;
					limbs[i].huntSpeed = 2f + (float)handChangeCounter;
					if (handChangeCounter < 10)
					{
						limbs[i].absoluteHuntPos = vector;
					}
					else if (handChangeCounter == 10)
					{
						limbs[i].FindGrip(snail.room, snail.mainBodyChunk.pos, limbs[i].pos, scaled * 0.9f, vector, 2, 2, true);
					}
				}
				else if (!Custom.DistLess(limbs[i].absoluteHuntPos, snail.mainBodyChunk.pos, scaled * 0.9f) && handChangeCounter > 10)
				{
					movingHand = 1 - movingHand;
					handChangeCounter = 0;
				}
			}
		}
		else
		{
			limbs[i].mode = Limb.Mode.Dangle;
		}
		if (!snail.Consious)
		{
			limbs[i].mode = Limb.Mode.Dangle;
		}
		if (limbs[i].mode == Limb.Mode.Dangle)
		{
			Limb limb = limbs[i];
			limb.vel.y = limb.vel.y - 0.9f * (1f - snail.mainBodyChunk.submersion);
		}
		limbs[i].Update();
		limbs[i].ConnectToPoint(snail.mainBodyChunk.pos, scaled, false, 0f, snail.mainBodyChunk.vel, 0.5f, 0f);
	}
```

!> Test