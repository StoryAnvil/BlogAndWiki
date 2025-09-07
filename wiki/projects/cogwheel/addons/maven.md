# Maven repo
To add Cogwheel Engine as gradle dependency you can use one of maven repos listed below.

## Cursemaven
Add cursemaven repository to your `build.gradle`
```groovy
repositories {
    maven {
        url "https://cursemaven.com"
    }
}
```

Then add go to [Cogwheel Engine curseforge](https://www.curseforge.com/minecraft/mc-mods/cogwheel-engine/files/all?page=1&pageSize=20) and select version of Cogwheel Engine you want to depend on.

![(curseforge file page image)](https://raw.githubusercontent.com/StoryAnvil/BlogAndWiki/master/wiki/projects/cogwheel/addons/cursemaven.png)

Now copy cursemaven snippet and paste it in your `build.gradle`:
```groovy
dependencies {
    implementation fg.deobf("curse.maven:cogwheel-engine-1305284:<file-id>")
}
```

## Modrinth Maven (recommended)
Add modrinth maven repository to your `build.gradle`
```groovy
repositories {
    maven {
        url = "https://api.modrinth.com/maven"
    }
}
```

Then go to [Cogwheel Engine modrinth](https://modrinth.com/mod/cogwheel-engine/versions) and select version you want to depend on.

Now you can add Cogwheel engine as gradle dependency in your `build.gradle`:
```groovy
dependencies {
    // you can get version number on modrinth version's page metadata section
    implementation fg.deobf("maven.modrinth:cogwheel-engine:<VERSION-NUMBER>")
}
```

## Reporting issues
If you have any problems with adding cogwheel engine as gradle dependency, you can report problems [on github](https://github.com/orgs/StoryAnvil/discussions/categories/general)