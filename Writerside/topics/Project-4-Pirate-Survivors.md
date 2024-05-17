# Project 4: Pirate Survivors

_In this entry, I discuss my multiplayer survivors-like group project, **Pirate Survivors**._

### Programming for Designers
In conversation with the designers of this project, we found that we believe it is important that the project be coded
in a way that functions as an API of sorts for designers to build on top off with more user-friendly solutions such as blueprints or the details panel.

With this problem in mind, I wanted to design a system that would allow designers (and any team member for that matter)
to create upgrades without code, and entirely new weapons solely in blueprint.

#### Architecture

Like Farm Survivors, I decided to use assets to carry data, making it easily editable.
Unreal's equivalent to Resources are [Data Assets](https://dev.epicgames.com/documentation/en-us/unreal-engine/data-assets-in-unreal-engine).

For example, here is the definition of a projectile's data:

```C++
UCLASS(BlueprintType)
class PIRATESURVIVORS_API UProjectileData : public UDataAsset
{
	GENERATED_BODY()

public:
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Projectile Data")
	FText Name = FText::FromString("Projectile");

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Projectile Data")
	FVector2D DamageMultiplier = FVector2D(1, 1);

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Projectile Data")
	FVector2D SpeedMultiplier = FVector2D(1, 1);

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Projectile Data")
	float LifeTime = 5;

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Projectile Data")
	UStaticMesh* Mesh;

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Projectile Data")
	TSubclassOf<class AProjectile> ProjectileSubclass;
	
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Projectile Data|Collision")
	bool bCollideWithWalls = true;
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Projectile Data|Collision")
	bool bCollideWithPlayers = true;
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Projectile Data|Collision")
	bool bCollideWithEnemies = true;
};
```

And creating one:

<video src="../videos/ProjDataDemo.mp4"/>

For weapon and player upgrades, I needed to be able to apply deltas to the properties.
For this, I use an array of structs that include the name of the property to change, and how it changes.

```C++
UENUM(BlueprintType)
enum class EUpgradeType : uint8
{
	Add       UMETA(DisplayName = "Add"),
	Multiply  UMETA(DisplayName = "Multiply")
};

USTRUCT(BlueprintType)
struct FStatUpgrade
{
	GENERATED_BODY()
		
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	FName PropertyName;
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	EUpgradeType UpgradeType;
	UPROPERTY(EditAnywhere, BlueprintReadWrite)
	float Delta;
};
```

This system is very flexible. It allows the user to change any property a player may have (health, damage, etc),
in multiple ways. They could use ``Add`` to have an absolute change, or ``Multiply`` for a percentage change.

For weapon functionality, I created a ``AWeaponFunctionality`` class that blueprints could inherit from and implement
a ``OnFire`` event. Furthermore, I implemented some basic utility functions:

```C++
UFUNCTION(BlueprintCallable, BlueprintPure, Category = "Weapon Utility")
TArray<AActor*> GetAllEnemies() const;

UFUNCTION(BlueprintCallable, BlueprintPure, Category = "Weapon Utility")
int GetEnemiesWithinWeaponRange(TArray<AEnemy*>& OutEnemiesInRange);

UFUNCTION(BlueprintCallable, BlueprintPure, Category = "Weapon Utility", meta = (WorldContext = "WorldContextObject"))
static int GetEnemiesWithinRange(UObject* WorldContextObject, FVector Origin, TArray<AEnemy*>& OutEnemiesInRange, float Radius);

UFUNCTION(BlueprintCallable, BlueprintPure, Category = "Weapon Utility", meta = (WorldContext = "WorldContextObject"))
static AEnemy* GetClosestEnemyWithinRange(UObject* WorldContextObject, FVector Origin, float Radius);

UFUNCTION(BlueprintCallable, BlueprintPure, Category = "Weapon Utility", meta = (WorldContext = "WorldContextObject"))
static int GetEnemiesWithinRangeSorted(UObject* WorldContextObject, FVector Origin, TArray<AEnemy*>& OutEnemiesInRange, float Radius);

UFUNCTION(BlueprintCallable, BlueprintPure, Category = "Weapon Utility")
AEnemy* GetClosestEnemyWithinWeaponRange();

UFUNCTION(BlueprintCallable, Category = "Weapon Utility")
AProjectile* SpawnProjectile(const FVector& Position, const FVector& Direction, UProjectileData* ProjectileType) const;
```

Together, this allowed designers to create new weapons solely in expressive blueprint code without touching C++.

#### Feedback

Generally, feedback from the team about the systems was very positive! There were a couple problems, however.

One piece of feedback received was that having to go into the class selection menu was annoying, so it would be
better to have each data type directly in the menu. I did some research into how assets work, and found [this documentation](https://github.com/ibbles/LearningUnrealEngine/blob/master/Creating%20new%20asset%20types.md):

![assetinfo.png](assetinfo.png)

With this, I was able to implement ``UAssetFactory`` subclasses that allowed designers much easier access to the data classes.

![AssetFactoryDemo.gif](AssetFactoryDemo.gif)

Another piece of feedback received is that a simple typo in the property name could cause an error in the upgrade, 
which is a very easy mistake to make. To solve this, I wanted to provide a set of options instead of an open 
text box. After [researching all the _UPROPERTY_ specifiers](https://benui.ca/unreal/uproperty/), I found this:

![getoptionsspecifier.png](getoptionsspecifier.png)

I could use this specifier to define a function that would return an array of all properties that could be edited:

```C++
TArray<FName> UPlayerStats::GetPropertyNames()
{
	TArray<FName> PropertyNames;
	for (TFieldIterator<FProperty> Prop(StaticClass()); Prop; ++Prop) {
		const FProperty* Property = *Prop;
		// Continue if the property is not a float
		if (Property->GetClass() != FFloatProperty::StaticClass()) continue;
		PropertyNames.Add(Property->GetFName());
	}
	return PropertyNames;
}
```

One issue with this approach is that I have to have separate structs for weapon and player stat upgrades, as their
GetOptions value needs to refer to different functions. This makes it harder to reuse code.

In practice, it looks like this:

![StatUpgradePropertyNameDemo.gif](StatUpgradePropertyNameDemo.gif)

This system makes the process of defining stat upgrades seamless and user-friendly. While it may mean nothing to
the player, I have learned that creating good developer tools is an important part of game programming; it speeds
up productivity and allows more members of the team to contribute.
