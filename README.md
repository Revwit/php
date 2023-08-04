# The whimsical Laravel Style Guide
Embark on a unique journey through Laravel! This isn't just another guide. It's flavored with hints of humor and designed for a refreshing coding experience. Dive in, share your insights, and let’s make writing and learning Laravel a tad more entertaining! 🎸📖

About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects

General PHP Rules

Code style must follow PSR-1 and PSR-12. Generally speaking, everything string-like that's not public-facing should use camelCase. Detailed examples on these are spread throughout the guide in their relevant sections.

Class defaults

By default, we don't use final. we assume that all our users know they are responsible for writing tests for any overwritten behaviour.

Void return types

If a method return nothing, it should be indicated with void. This makes it more clear to the users of your code what your intention was when writing it.

Typed properties

You should type a property whenever possible. Don't use a docblock.

// good
class Foo
{
    public string $bar;
}

// bad
class Foo
{
    /** @var string */
    public $bar;
}
Docblocks

Don't use docblocks for methods that can be fully type hinted (unless you need a description).

Only add a description when it provides more context than the method signature itself. Use full sentences for descriptions, including a period at the end.

// Good
class Url
{
    public static function fromString(string $url): Url
    {
        // ...
    }
}

// Bad: The description is redundant, and the method is fully type-hinted.
class Url
{
    /**
     * Create a url from a string.
     *
     * @param string $url
     *
     * @return \Spatie\Url\Url
     */
    public static function fromString(string $url): Url
    {
        // ...
    }
}
Always use fully qualified class names in docblocks.

// Good

/**
 * @param string $url
 *
 * @return \Spatie\Url\Url
 */

// Bad

/**
 * @param string $foo
 *
 * @return Url
 */
Docblocks for class variables are required, as there's currently no other way to typehint these.

// Good

class Foo
{
    /** @var \Spatie\Url\Url */
    private $url;

    /** @var string */
    private $name;
}

// Bad

class Foo
{
    private $url;
    private $name;
}
When possible, docblocks should be written on one line.

// Good

/** @var string */
/** @test */

// Bad

/**
 * @test
 */
If a variable has multiple types, the most common occurring type should be first.

// Good

/** @var \Spatie\Goo\Bar|null */

// Bad

/** @var null|\Spatie\Goo\Bar */
Strings

When possible prefer string interpolation above sprintf and the . operator.

// Good
$greeting = "Hi, I am {$name}.";
// Bad
$greeting = 'Hi, I am ' . $name . '.';
Ternary operators

Every portion of a ternary expression should be on its own line unless it's a really short expression.

// Good
$result = $object instanceof Model
    ? $object->name
    : 'A default value';

$name = $isFoo ? 'foo' : 'bar';

// Bad
$result = $object instanceof Model ?
    $object->name :
   'A default value';
If statements

Bracket position

Always use curly brackets.

// Good
if ($condition) {
   ...
}

// Bad
if ($condition) ...
Happy path

Generally a function should have its unhappy path first and its happy path last. In most cases this will cause the happy path being in an unindented part of the function which makes it more readable.

// Good

if (! $goodCondition) {
  throw new Exception;
}

// do work
// Bad

if ($goodCondition) {
 // do work
}

throw new Exception;
Avoid else

In general, else should be avoided because it makes code less readable. In most cases it can be refactored using early returns. This will also cause the happy path to go last, which is desirable.

// Good

if (! $conditionBA) {
   // conditionB A failed
   
   return;
}

if (! $conditionB) {
   // conditionB A passed, B failed
   
   return;
}

// condition A and B passed
// Bad

if ($conditionA) {
   if ($conditionB) {
      // condition A and B passed
   }
   else {
     // conditionB A passed, B failed
   }
}
else {
   // conditionB A failed
}
Compound ifs

In general, separate if statements should be preferred over a compound condition. This makes debugging code easier.

// Good
if (! $conditionA) {
   return;
}

if (! $conditionB) {
   return;
}

if (! $conditionC) {
   return;
}

// do stuff
// bad
if ($conditionA && $conditionB && $conditionC) {
  // do stuff
}
Comments

Comments should be avoided as much as possible by writing expressive code. If you do need to use a comment, format it like this:

// There should be a space before a single line comment.

/*
 * If you need to explain a lot you can use a comment block. Notice the
 * single * on the first line. Comment blocks don't need to be three
 * lines long or three characters shorter than the previous line.
 */
Whitespace

Statements should have to breathe. In general always add blank lines between statements, unless they're a sequence of single-line equivalent operations. This isn't something enforceable, it's a matter of what looks best in its context.

// Good
public function getPage($url)
{
    $page = $this->pages()->where('slug', $url)->first();

    if (! $page) {
        return null;
    }

    if ($page['private'] && ! Auth::check()) {
        return null;
    }

    return $page;
}

// Bad: Everything's cramped together.
public function getPage($url)
{
    $page = $this->pages()->where('slug', $url)->first();
    if (! $page) {
        return null;
    }
    if ($page['private'] && ! Auth::check()) {
        return null;
    }
    return $page;
}
// Good: A sequence of single-line equivalent operations.
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->increments('id');
        $table->string('name');
        $table->string('email')->unique();
        $table->string('password');
        $table->rememberToken();
        $table->timestamps();
    });
}
Don't add any extra empty lines between {} brackets.

// Good
if ($foo) {
    $this->foo = $foo;
}

// Bad
if ($foo) {

    $this->foo = $foo;

}
Configuration

Configuration files must use kebab-case.

config/
  pdf-generator.php
Configuration keys must use snake_case.

// config/pdf-generator.php
return [
    'chrome_path' => env('CHROME_PATH'),
];
Avoid using the env helper outside of configuration files. Create a configuration value from the env variable like above.

Artisan commands

The names given to artisan commands should all be kebab-cased.

# Good
php artisan delete-old-records

# Bad
php artisan deleteOldRecords
A command should always give some feedback on what the result is. Minimally you should let the handle method spit out a comment at the end indicating that all went well.

// in a Command
public function handle()
{
    // do some work

    $this->comment('All ok!');
}
If possible use a descriptive success message eg. Old records deleted.

Routing

Public-facing urls must use kebab-case.

https://spatie.be/open-source
https://spatie.be/jobs/front-end-developer
Route names must use camelCase.

Route::get('open-source', 'OpenSourceController@index')->name('openSource');
<a href="{{ route('openSource') }}">
    Open Source
</a>
All routes have an http verb, that's why we like to put the verb first when defining a route. It makes a group of routes very readable. Any other route options should come after it.

// good: all http verbs come first
Route::get('/', 'HomeController@index')->name('home');
Route::get('open-source', 'OpenSourceController@index')->name('openSource');

// bad: http verbs not easily scannable
Route::name('home')->get('/', 'HomeController@index');
Route::name('openSource')->get('OpenSourceController@index');
Route parameters should use camelCase.

Route::get('news/{newsItem}', 'NewsItemsController@index');
A route url should not start with / unless the url would be an empty string.

// good
Route::get('/', 'HomeController@index');
Route::get('open-source', 'OpenSourceController@index');

//bad
Route::get('', 'HomeController@index');
Route::get('/open-source', 'OpenSourceController@index');
Controllers

Controllers that control a resource must use the plural resource name.

class PostsController
{
    // ...
}
Try to keep controllers simple and stick to the default CRUD keywords (index, create, store, show, edit, update, destroy). Extract a new controller if you need other actions.

In the following example, we could have PostsController@favorite, and PostsController@unfavorite, or we could extract it to a separate FavoritePostsController.

class PostsController
{
    public function create()
    {
        // ...
    }

    // ...

    public function favorite(Post $post)
    {
        request()->user()->favorites()->attach($post);

        return response(null, 200);
    }

    public function unfavorite(Post $post)
    {
        request()->user()->favorites()->detach($post);

        return response(null, 200);
    }
}
Here we fall back to default CRUD words, store and destroy.

class FavoritePostsController
{
    public function store(Post $post)
    {
        request()->user()->favorites()->attach($post);

        return response(null, 200);
    }

    public function destroy(Post $post)
    {
        request()->user()->favorites()->detach($post);

        return response(null, 200);
    }
}
This is a loose guideline that doesn't need to be enforced.

Views

View files must use camelCase.

resources/
  views/
    openSource.blade.php
class OpenSourceController
{
    public function index() {
        return view('openSource');
    }
}
Validation

When using multiple rules for one field in a form request, avoid using |, always use array notation. Using an array notation will make it easier to apply custom rule classes to a field.

// good
public function rules()
{
    return [
        'email' => ['required', 'email'],
    ];
}

// bad
public function rules()
{
    return [
        'email' => 'required|email',
    ];
}
All custom validation rules must use snake_case:

Validator::extend('organisation_type', function ($attribute, $value) {
    return OrganisationType::isValid($value);
});
Blade Templates

Indent using four spaces.

<a href="/open-source">
    Open Source
</a>
Don't add spaces after control structures.

@if($condition)
    Something
@endif
Authorization

Policies must use camelCase.

Gate::define('editPost', function ($user, $post) {
    return $user->id == $post->user_id;
});
@can('editPost', $post)
    <a href="{{ route('posts.edit', $post) }}">
        Edit
    </a>
@endcan
Try to name abilities using default CRUD words. One exception: replace show with view. A server shows a resource, a user views it.

Translations

Translations must be rendered with the __ function. We prefer using this over @lang in Blade views because __ can be used in both Blade views and regular PHP code. Here's an example:

<h2>{{ __('newsletter.form.title') }}</h2>

{!! __('newsletter.form.description') !!}
Naming Classes

Naming things is often seen as one of the harder things in programming. That's why we've established some high level guidelines for naming classes.

Controllers

Generally controllers are named by the plural form of their corresponding resource and a Controller suffix. This is to avoid naming collisions with models that are often equally named.

e.g. UsersController or EventDaysController

When writing non-resourceful controllers you might come across invokable controllers that perform a single action. These can be named by the action they perform again suffixed by Controller.

e.g. PerformCleanupController

Resources (and transformers)

Both Eloquent resources and Fractal transformers are plural resources suffixed with Resource or Transformer accordingly. This is to avoid naming collisions with models.

Jobs

A job's name should describe its action.

E.g. CreateUser or PerformDatabaseCleanup

Events

Events will often be fired before or after the actual event. This should be very clear by the tense used in their name.

E.g. ApprovingLoan before the action is completed and LoanApproved after the action is completed.

Listeners

Listeners will perform an action based on an incoming event. Their name should reflect that action with a Listener suffix. This might seem strange at first but will avoid naming collisions with jobs.

E.g. SendInvitationMailListener

Commands

To avoid naming collisions we'll suffix commands with Command, so they are easiliy distinguisable from jobs.

e.g. PublishScheduledPostsCommand

Mailables

Again to avoid naming collisions we'll suffix mailables with Mail, as they're often used to convey an event, action or question.

e.g. AccountActivatedMail or NewEventMail
