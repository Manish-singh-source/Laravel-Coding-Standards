```
use Illuminate\Support\Facades\Gate;
// 
public function boot()
{
    // Implicitly grant "Super Admin" role all permissions
    // This works in the app by using gate-related functions like auth()->user->can() and @can()
    Gate::before(function ($user, $ability) {
        return $user->hasRole('Super Admin') ? true : null;
    });
}
```
