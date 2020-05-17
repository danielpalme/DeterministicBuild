# Deterministic build

## Non deterministic

### Settings
`Directory.Build.props`:
```xml
  <PropertyGroup>
    <!-- <ContinuousIntegrationBuild>true</ContinuousIntegrationBuild>
    <Deterministic>true</Deterministic> -->
  </PropertyGroup>
```

### Test result
`dotnet test /p:CollectCoverage=true /p:DeterministicSourcePaths=true`:  
```
+---------------+------+--------+--------+
| Module        | Line | Branch | Method |
+---------------+------+--------+--------+
| ClassLibrary1 | 100% | 100%   | 100%   |
+---------------+------+--------+--------+

+---------+------+--------+--------+
|         | Line | Branch | Method |
+---------+------+--------+--------+
| Total   | 100% | 100%   | 100%   |
+---------+------+--------+--------+
| Average | 100% | 100%   | 100%   |
+---------+------+--------+--------+
```
**Results:**  
`Average` results are correct in table above
`DeterministicBuild\XUnitTestProject1\coverage.json` contains correct results  
`DeterministicBuild\XUnitTestProject1\bin\Debug\netcoreapp3.1\CoverletSourceRootsMapping` does not exist (makes sense)

## Deterministic

### Settings
`Directory.Build.props`:
```xml
  <PropertyGroup>
    <ContinuousIntegrationBuild>true</ContinuousIntegrationBuild>
    <Deterministic>true</Deterministic>
  </PropertyGroup>
```

### Test result
`dotnet test /p:CollectCoverage=true /p:DeterministicSourcePaths=true`:  
```
+--------+------+--------+--------+
| Module | Line | Branch | Method |
+--------+------+--------+--------+

+---------+------+--------+--------+
|         | Line | Branch | Method |
+---------+------+--------+--------+
| Total   | 100% | 100%   | 100%   |
+---------+------+--------+--------+
| Average | NaN% | NaN%   | NaN%   |
+---------+------+--------+--------+
```

**Results:**  
`Average` results are missing in the second table. Assembly `ClassLibrary1` is missing in first table  
`DeterministicBuild\XUnitTestProject1\coverage.json` is empty   
`DeterministicBuild\XUnitTestProject1\bin\Debug\netcoreapp3.1\CoverletSourceRootsMapping` (does not exist does not make sense)
