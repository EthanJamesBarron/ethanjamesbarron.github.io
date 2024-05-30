# Limbs
Limbs are an **integral** thing for creatures in Rain World. Think of them like small brains. They reside in **`GraphicsModules`**
The name is pretty much self-explanatory. The idea is you have an array of limbs to start off with. Preferably you'd set this up in the `constructor` of your creature
We're going to be looking at Eggbugs here

```cs
	legs = new Limb[2, 2]; // Taken from eggbugs. This 2D array represents the back and front legs on an eggbug
	for (int i = 0; i < legs.GetLength(0); i++)
	{
		for (int j = 0; j < legs.GetLength(1); j++) // For each 
		{
			legs[i, j] = new Limb(this, bug.mainBodyChunk, i * 2 + j, 0.1f, 0.7f, 0.99f, 17f, 0.8f);
		}
	}
```