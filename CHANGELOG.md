# Change log

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/) and this project adheres to [Semantic Versioning](http://semver.org/).

## [v2.0] - 2016-09-13
### Added
- Landlord now supports Lumen (5.2+) out of the box.
- Landlord now uses Laravel's anonymous global scopes, so you can disable scoping for one or more individual tenants for a single query using `Model::withoutGlobalScope('tenant_column')`, or `Model::withoutGlobalScopes(['tenant_column_a', 'tenant_column_b'])`. 

    **Note:** `Model::allTenants()` still returns a query with *none* of the tenant scopes applied.
    
- You can now pass a Model instance to `addTenant()`. Landlord will use Eloquent's `getForeignKey()` method as the tenant column name.

### Changed
- Renamed `LandlordFacade` → `Landlord`.
- Renamed `BelongsToTenant` → `BelongsToTenants` (plural). 

    **Note:** You will have to update your use statements in scoped models.
   
- Renamed `TenantModelNotFoundException` → `ModelNotFoundForTenantException`. Make sure to update any `catch` statements.
- Renamed `Landlord` → `TenantManager`. 

    **Note:** You will have to update any places you're injecting an instance:

```php
//Before
public function __construct(\HipsterJazzbo\Landlord\Landlord $landlord) {
    $this->landlord = $landlord;
}

// After
public function __construct(\HipsterJazzbo\Landlord\TenantManager $landlord) {
    $this->landlord = $landlord;
}
```
        
- `TenantManager` now uses an `\Illuminate\Support\Collection` instance to manage tenants internally. This has cleaned up the code a lot. 

    **Note** `getTenants()` now returns the `Collection` instance instead of an array. If you need a plain array of tenants, you may call `Landlord::getTenants()->all()`.
- The service provider no longer registers the `Landlord` facade for you. You'll need to do it in your `config/app.php` if you weren't already.
- Landlord now actually checks for non-tenanted existence before throwing a `ModelNotFoundForTenantException`.