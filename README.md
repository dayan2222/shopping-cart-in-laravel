# shopping-cart-in-laravel
Shopping Cart in Laravel
First install the laravel installer

    $ composer global require laravel/installer
    
Then, 
  
    $ composer create-project laravel/laravel shopping-cart

After a successfull installation run the script below to add key in .env file of root folder in laravel app.

    $ php artisan key:generate

Now make model and migration named it Product

    $ php artisan make:model Product -m

Go to migration of Product and add some rows in up function.

      <?php

      use Illuminate\Database\Migrations\Migration;
      use Illuminate\Database\Schema\Blueprint;
      use Illuminate\Support\Facades\Schema;

      class CreateProductsTable extends Migration
      {
          /**
           * Run the migrations.
           *
           * @return void
           */
          public function up()
          {
              Schema::create('products', function (Blueprint $table) {
                  $table->id();
                  $table->string("name", 255)->nullable();
                  $table->text("description")->nullable();
                  $table->string("photo", 255)->nullable();
                  $table->decimal("price", 6, 2);
                  $table->timestamps();
              });
          }

          /**
           * Reverse the migrations.
           *
           * @return void
           */
          public function down()
          {
              Schema::dropIfExists('products');
          }
      }
      
Now let’s create a Seeder to populate our products table with some data to work with. Run this command to generate a seeder.
    
     $ php artisan make:seed ProductsSeeder

Go to database>seeds>ProductsSeeder. Add following.
     
     use Illuminate\Support\Facades\DB; 
     
Then inside run() function add this code.     
    
             DB::table('products')->insert([
            'name' => 'Samsung Galaxy S9',
            'description' => 'A brand new, sealed Lilac Purple Verizon Global Unlocked Galaxy S9 by Samsung. This is an upgrade. Clean ESN and activation ready.',
            'photo' => 'https://i.ebayimg.com/00/s/ODY0WDgwMA==/z/9S4AAOSwMZRanqb7/$_35.JPG?set_id=89040003C1',
            'price' => 698.88
         ]);
         DB::table('products')->insert([
             'name' => 'Apple iPhone X',
             'description' => 'GSM & CDMA FACTORY UNLOCKED! WORKS WORLDWIDE! FACTORY UNLOCKED. iPhone x 64gb. iPhone 8 64gb. iPhone 8 64gb. iPhone X with iOS 11.',
             'photo' => 'https://i.ebayimg.com/00/s/MTYwMFg5OTU=/z/9UAAAOSwFyhaFXZJ/$_35.JPG?set_id=89040003C1',
             'price' => 983.00
         ]);
         DB::table('products')->insert([
             'name' => 'Google Pixel 2 XL',
             'description' => 'New condition • No returns, but backed by eBay Money back guarantee',
             'photo' => 'https://i.ebayimg.com/00/s/MTYwMFg4MzA=/z/G2YAAOSwUJlZ4yQd/$_35.JPG?set_id=89040003C1',
             'price' => 675.00
         ]);
         DB::table('products')->insert([
             'name' => 'LG V10 H900',
             'description' => 'NETWORK Technology GSM. Protection Corning Gorilla Glass 4. MISC Colors Space Black, Luxe White, Modern Beige, Ocean Blue, Opal Blue. SAR EU 0.59               W/kg (head).',
             'photo' => 'https://i.ebayimg.com/00/s/NjQxWDQyNA==/z/VDoAAOSwgk1XF2oo/$_35.JPG?set_id=89040003C1',
             'price' => 159.99
         ]);
         DB::table('products')->insert([
             'name' => 'Huawei Elate',
             'description' => 'Cricket Wireless - Huawei Elate. New Sealed Huawei Elate Smartphone.',
             'photo' => 'https://ssli.ebayimg.com/images/g/aJ0AAOSw7zlaldY2/s-l640.jpg',
             'price' => 68.00
         ]);
         DB::table('products')->insert([
             'name' => 'HTC One M10',
             'description' => 'The device is in good cosmetic condition and will show minor scratches and/or scuff marks.',
             'photo' => 'https://i.ebayimg.com/images/g/u-kAAOSw9p9aXNyf/s-l500.jpg',
             'price' => 129.99
         ]);
         
         
Now run this command to seed your data.         

      $ php artisan db:seed --class=ProductsSeeder
      
Check the table Products where you can see the populated table with data.

## Controller & Views

      $ php artisan make:controller ProductsController
      
Add the following code in Products Controller.
     
    public function index()
    {
        return view('products');
    }
    public function cart()
    {
        return view('cart');
    }
    public function addToCart($id)
    {
    }
    
Next we will create three views with dummy data:

  **layout.blade.php:** this is the main layout view.
  **products.blade.php:** this is the view that will display our products.
  **cart.blade.php: this** is the cart view.
  In the **layout.blade.php** copy and paste this code. 

    <!DOCTYPE html>
    <html>
    <head>

        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />

        <title>@yield('title')</title>

        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css">
        <link rel="stylesheet" type="text/css" href="{{ asset('css/style.css') }}">

        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js"></script>

    </head>
    <body>

    <div class="container">

        <div class="row">
            <div class="col-lg-12 col-sm-12 col-12 main-section">
                <div class="dropdown">
                    <button type="button" class="btn btn-info" data-toggle="dropdown">
                        <i class="fa fa-shopping-cart" aria-hidden="true"></i> Cart <span class="badge badge-pill badge-danger">3</span>
                    </button>
                    <div class="dropdown-menu">
                        <div class="row total-header-section">
                            <div class="col-lg-6 col-sm-6 col-6">
                                <i class="fa fa-shopping-cart" aria-hidden="true"></i> <span class="badge badge-pill badge-danger">3</span>
                            </div>
                            <div class="col-lg-6 col-sm-6 col-6 total-section text-right">
                                <p>Total: <span class="text-info">$2,978.24</span></p>
                            </div>
                        </div>
                        <div class="row cart-detail">
                            <div class="col-lg-4 col-sm-4 col-4 cart-detail-img">
                                <img src="https://images-na.ssl-images-amazon.com/images/I/811OyrCd5hL._SX425_.jpg">
                            </div>
                            <div class="col-lg-8 col-sm-8 col-8 cart-detail-product">
                                <p>Sony DSC-RX100M..</p>
                                <span class="price text-info"> $250.22</span> <span class="count"> Quantity:01</span>
                            </div>
                        </div>
                        <div class="row cart-detail">
                            <div class="col-lg-4 col-sm-4 col-4 cart-detail-img">
                                <img src="https://cdn2.gsmarena.com/vv/pics/blu/blu-vivo-48-1.jpg">
                            </div>
                            <div class="col-lg-8 col-sm-8 col-8 cart-detail-product">
                                <p>Vivo DSC-RX100M..</p>
                                <span class="price text-info"> $500.40</span> <span class="count"> Quantity:01</span>
                            </div>
                        </div>
                        <div class="row cart-detail">
                            <div class="col-lg-4 col-sm-4 col-4 cart-detail-img">
                                <img src="https://static.toiimg.com/thumb/msid-55980052,width-640,resizemode-4/55980052.jpg">
                            </div>
                            <div class="col-lg-8 col-sm-8 col-8 cart-detail-product">
                                <p>Lenovo DSC-RX100M..</p>
                                <span class="price text-info"> $445.78</span> <span class="count"> Quantity:01</span>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-lg-12 col-sm-12 col-12 text-center checkout">
                                <a href="{{ url('cart') }}" class="btn btn-primary btn-block">View all</a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="container page">
        @yield('content')
    </div>


    @yield('scripts')

    </body>
    </html>

**products.blade.php**

    @extends('layout')

    @section('title', 'Products')

    @section('content')

        <div class="container products">

            <div class="row">

                <div class="col-xs-18 col-sm-6 col-md-3">
                    <div class="thumbnail">
                        <img src="http://placehold.it/500x300" alt="">
                        <div class="caption">
                            <h4>Product name</h4>
                            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Facere, soluta, eligendi doloribus sunt minus amet sit debitis repellat. Consectetur, culpa itaque odio similique suscipit</p>
                            <p><strong>Price: </strong> 567.7$</p>
                            <p class="btn-holder"><a href="#" class="btn btn-warning btn-block text-center" role="button">Add to cart</a> </p>
                        </div>
                    </div>
                </div>

                <div class="col-xs-18 col-sm-6 col-md-3">
                    <div class="thumbnail">
                        <img src="http://placehold.it/500x300" alt="">
                        <div class="caption">
                            <h4>Product name</h4>
                            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Facere, soluta, eligendi doloribus sunt minus amet sit debitis repellat. Consectetur, culpa itaque odio similique suscipit</p>
                            <p><strong>Price: </strong> 567.7$</p>
                            <p class="btn-holder"><a href="#" class="btn btn-warning btn-block text-center" role="button">Add to cart</a> </p>
                        </div>
                    </div>
                </div>

                <div class="col-xs-18 col-sm-6 col-md-3">
                    <div class="thumbnail">
                        <img src="http://placehold.it/500x300" alt="">
                        <div class="caption">
                            <h4>Product name</h4>
                            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Facere, soluta, eligendi doloribus sunt minus amet sit debitis repellat. Consectetur, culpa itaque odio similique suscipit</p>
                            <p><strong>Price: </strong> 567.7$</p>
                            <p class="btn-holder"><a href="#" class="btn btn-warning btn-block text-center" role="button">Add to cart</a> </p>
                        </div>
                    </div>
                </div>

                <div class="col-xs-18 col-sm-6 col-md-3">
                    <div class="thumbnail">
                        <img src="http://placehold.it/500x300" alt="">
                        <div class="caption">
                            <h4>Product name</h4>
                            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Facere, soluta, eligendi doloribus sunt minus amet sit debitis repellat. Consectetur, culpa itaque odio similique suscipit</p>
                            <p><strong>Price: </strong> 567.7$</p>
                            <p class="btn-holder"><a href="#" class="btn btn-warning btn-block text-center" role="button">Add to cart</a> </p>
                        </div>
                    </div>
                </div>

            </div><!-- End row -->

        </div>

    @endsection
    
    
**cart.blade.php**
    
    @extends('layout')

    @section('title', 'Cart')

    @section('content')

        <table id="cart" class="table table-hover table-condensed">
            <thead>
            <tr>
                <th style="width:50%">Product</th>
                <th style="width:10%">Price</th>
                <th style="width:8%">Quantity</th>
                <th style="width:22%" class="text-center">Subtotal</th>
                <th style="width:10%"></th>
            </tr>
            </thead>
            <tbody>
            <tr>
                <td data-th="Product">
                    <div class="row">
                        <div class="col-sm-3 hidden-xs"><img src="http://placehold.it/100x100" alt="..." class="img-responsive"/></div>
                        <div class="col-sm-9">
                            <h4 class="nomargin">Product 1</h4>
                            <p>Quis aute iure reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Lorem ipsum dolor sit amet.</p>
                        </div>
                    </div>
                </td>
                <td data-th="Price">$1.99</td>
                <td data-th="Quantity">
                    <input type="number" class="form-control text-center" value="1">
                </td>
                <td data-th="Subtotal" class="text-center">1.99</td>
                <td class="actions" data-th="">
                    <button class="btn btn-info btn-sm"><i class="fa fa-refresh"></i></button>
                    <button class="btn btn-danger btn-sm"><i class="fa fa-trash-o"></i></button>
                </td>
            </tr>
            </tbody>
            <tfoot>
            <tr class="visible-xs">
                <td class="text-center"><strong>Total 1.99</strong></td>
            </tr>
            <tr>
                <td><a href="{{ url('/') }}" class="btn btn-warning"><i class="fa fa-angle-left"></i> Continue Shopping</a></td>
                <td colspan="2" class="hidden-xs"></td>
                <td class="hidden-xs text-center"><strong>Total $1.99</strong></td>
            </tr>
            </tfoot>
        </table>

    @endsection
    
**style.css**

    .main-section{
        background-color: #F8F8F8;
    }
    .dropdown{
        float:right;
        padding-right: 30px;
    }
    .btn{
        border:0px;
        margin:10px 0px;
        box-shadow:none !important;
    }
    .dropdown .dropdown-menu{
        padding:20px;
        top:30px !important;
        width:350px !important;
        left:-110px !important;
        box-shadow:0px 5px 30px black;
    }
    .total-header-section{
        border-bottom:1px solid #d2d2d2;
    }
    .total-section p{
        margin-bottom:20px;
    }
    .cart-detail{
        padding:15px 0px;
    }
    .cart-detail-img img{
        width:100%;
        height:100%;
        padding-left:15px;
    }
    .cart-detail-product p{
        margin:0px;
        color:#000;
        font-weight:500;
    }
    .cart-detail .price{
        font-size:12px;
        margin-right:10px;
        font-weight:500;
    }
    .cart-detail .count{
        color:#C2C2DC;
    }
    .checkout{
        border-top:1px solid #d2d2d2;
        padding-top: 15px;
    }
    .checkout .btn-primary{
        border-radius:50px;
        height:50px;
    }
    .dropdown-menu:before{
        content: " ";
        position:absolute;
        top:-20px;
        right:50px;
        border:10px solid transparent;
        border-bottom-color:#fff;
    }

    .thumbnail {
        position: relative;
        padding: 0px;
        margin-bottom: 20px;
    }

    .thumbnail img {
        width: 100%;
    }

    .thumbnail .caption{
        margin: 7px;
    }

    .page {
        margin-top: 50px;
    }

    .btn-holder{
        text-align: center;
    }

    .table>tbody>tr>td, .table>tfoot>tr>td{
        vertical-align: middle;
    }
    @media screen and (max-width: 600px) {
        table#cart tbody td .form-control{
            width:20%;
            display: inline !important;
        }
        .actions .btn{
            width:36%;
            margin:1.5em 0;
        }

        .actions .btn-info{
            float:left;
        }
        .actions .btn-danger{
            float:right;
        }

        table#cart thead { display: none; }
        table#cart tbody td { display: block; padding: .6rem; min-width:320px;}
        table#cart tbody tr td:first-child { background: #333; color: #fff; }
        table#cart tbody td:before {
            content: attr(data-th); font-weight: bold;
            display: inline-block; width: 8rem;
        }
        table#cart tfoot td{display:block; }
        table#cart tfoot td .btn{display:block;}

    }
    
## Routes

At this point go to routes>web.php and add the following routes:

    <?php

    use Illuminate\Support\Facades\Route;
    use App\Http\Controllers\ProductsController;

    /*
    |--------------------------------------------------------------------------
    | Web Routes
    |--------------------------------------------------------------------------
    |
    | Here is where you can register web routes for your application. These
    | routes are loaded by the RouteServiceProvider within a group which
    | contains the "web" middleware group. Now create something great!
    |
    */

    Route::get('/', [ProductsController::class, 'index']);
    Route::get('cart', [ProductsController::class, 'cart']);
    Route::get('add-to-cart/{id}', [ProductsController::class, 'addToCart']);
    Route::patch('update-cart',  [ProductsController::class, 'update']);

    Route::delete('remove-from-cart',  [ProductsController::class, 'remove']);
    
## Displaying Products
Declare model in COntroller 
    
    $ use App\Models\Product;

Go to ProductsController.php in the index function add the below code

    public function index()
        {
            $products = Product::all();

            return view('products', compact('products'));
        }
Now go to resources>views>**products.blade.php** and replace the dummy data with the below code.

      @extends('layout')

        @section('title', 'Products')

        @section('content')

            <div class="container products">

                <div class="row">

                    @foreach($products as $product)
                        <div class="col-xs-18 col-sm-6 col-md-3">
                            <div class="thumbnail">
                                <img src="{{ $product->photo }}" width="500" height="300">
                                <div class="caption">
                                    <h4>{{ $product->name }}</h4>
                                    <p>{{ str_limit(strtolower($product->description), 50) }}</p>
                                    <p><strong>Price: </strong> {{ $product->price }}$</p>
                                    <p class="btn-holder"><a href="{{ url('add-to-cart/'.$product->id) }}" class="btn btn-warning btn-block text-center" role="button">Add to cart</a> </p>
                                </div>
                            </div>
                        </div>
                    @endforeach

                </div><!-- End row -->

            </div>

        @endsection

## Processing the Cart
Now its time to add logic for when user clicks on add to cart button. We will do this in the addToCart function in ProductsController.php and we will store the cart item using laravel session.

      public function addToCart($id)
          {
              $product = Product::find($id);

              if(!$product) {

                  abort(404);

              }

              $cart = session()->get('cart');

              // if cart is empty then this the first product
              if(!$cart) {

                  $cart = [
                          $id => [
                              "name" => $product->name,
                              "quantity" => 1,
                              "price" => $product->price,
                              "photo" => $product->photo
                          ]
                  ];

                  session()->put('cart', $cart);

                  return redirect()->back()->with('success', 'Product added to cart successfully!');
              }

              // if cart not empty then check if this product exist then increment quantity
              if(isset($cart[$id])) {

                  $cart[$id]['quantity']++;

                  session()->put('cart', $cart);

                  return redirect()->back()->with('success', 'Product added to cart successfully!');

              }

              // if item not exist in cart then add to cart with quantity = 1
              $cart[$id] = [
                  "name" => $product->name,
                  "quantity" => 1,
                  "price" => $product->price,
                  "photo" => $product->photo
              ];

              session()->put('cart', $cart);

              return redirect()->back()->with('success', 'Product added to cart successfully!');
          }
          
## Displaying the cart contents
At this point we have a cart with products let’s display it in the cart view.

Go to resources>views>**cart.blade.php** and update the view with below code:

      @extends('layout')

      @section('title', 'Cart')

      @section('content')

          <table id="cart" class="table table-hover table-condensed">
              <thead>
              <tr>
                  <th style="width:50%">Product</th>
                  <th style="width:10%">Price</th>
                  <th style="width:8%">Quantity</th>
                  <th style="width:22%" class="text-center">Subtotal</th>
                  <th style="width:10%"></th>
              </tr>
              </thead>
              <tbody>

              <?php $total = 0 ?>

              @if(session('cart'))
                  @foreach(session('cart') as $id => $details)

                      <?php $total += $details['price'] * $details['quantity'] ?>

                      <tr>
                          <td data-th="Product">
                              <div class="row">
                                  <div class="col-sm-3 hidden-xs"><img src="{{ $details['photo'] }}" width="100" height="100" class="img-responsive"/></div>
                                  <div class="col-sm-9">
                                      <h4 class="nomargin">{{ $details['name'] }}</h4>
                                  </div>
                              </div>
                          </td>
                          <td data-th="Price">${{ $details['price'] }}</td>
                          <td data-th="Quantity">
                              <input type="number" value="{{ $details['quantity'] }}" class="form-control quantity" />
                          </td>
                          <td data-th="Subtotal" class="text-center">${{ $details['price'] * $details['quantity'] }}</td>
                          <td class="actions" data-th="">
                              <button class="btn btn-info btn-sm update-cart" data-id="{{ $id }}"><i class="fa fa-refresh"></i></button>
                              <button class="btn btn-danger btn-sm remove-from-cart" data-id="{{ $id }}"><i class="fa fa-trash-o"></i></button>
                          </td>
                      </tr>
                  @endforeach
              @endif

              </tbody>
              <tfoot>
              <tr class="visible-xs">
                  <td class="text-center"><strong>Total {{ $total }}</strong></td>
              </tr>
              <tr>
                  <td><a href="{{ url('/') }}" class="btn btn-warning"><i class="fa fa-angle-left"></i> Continue Shopping</a></td>
                  <td colspan="2" class="hidden-xs"></td>
                  <td class="hidden-xs text-center"><strong>Total ${{ $total }}</strong></td>
              </tr>
              </tfoot>
          </table>

      @endsection
      
And also update the main layout view **layout.blade.php**

      <!DOCTYPE html>
      <html>
      <head>

          <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />

          <title>@yield('title')</title>

          <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css">
          <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css">
          <link rel="stylesheet" type="text/css" href="{{ asset('css/style.css') }}">

          <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
          <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js"></script>
          <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js"></script>

      </head>
      <body>

      <div class="container">

          <div class="row">
              <div class="col-lg-12 col-sm-12 col-12 main-section">
                  <div class="dropdown">
                      <button type="button" class="btn btn-info" data-toggle="dropdown">
                          <i class="fa fa-shopping-cart" aria-hidden="true"></i> Cart <span class="badge badge-pill badge-danger">{{ count((array) session('cart')) }}</span>
                      </button>
                      <div class="dropdown-menu">
                          <div class="row total-header-section">
                              <div class="col-lg-6 col-sm-6 col-6">
                                  <i class="fa fa-shopping-cart" aria-hidden="true"></i> <span class="badge badge-pill badge-danger">{{ count((array) session('cart')) }}</span>
                              </div>

                              <?php $total = 0 ?>
                              @foreach((array) session('cart') as $id => $details)
                                  <?php $total += $details['price'] * $details['quantity'] ?>
                              @endforeach

                              <div class="col-lg-6 col-sm-6 col-6 total-section text-right">
                                  <p>Total: <span class="text-info">$ {{ $total }}</span></p>
                              </div>
                          </div>

                          @if(session('cart'))
                              @foreach(session('cart') as $id => $details)
                                  <div class="row cart-detail">
                                      <div class="col-lg-4 col-sm-4 col-4 cart-detail-img">
                                          <img src="{{ $details['photo'] }}" />
                                      </div>
                                      <div class="col-lg-8 col-sm-8 col-8 cart-detail-product">
                                          <p>{{ $details['name'] }}</p>
                                          <span class="price text-info"> ${{ $details['price'] }}</span> <span class="count"> Quantity:{{ $details['quantity'] }}</span>
                                      </div>
                                  </div>
                              @endforeach
                          @endif
                          <div class="row">
                              <div class="col-lg-12 col-sm-12 col-12 text-center checkout">
                                  <a href="{{ url('cart') }}" class="btn btn-primary btn-block">View all</a>
                              </div>
                          </div>
                      </div>
                  </div>
              </div>
          </div>
      </div>

      <div class="container page">
          @yield('content')
      </div>

      @yield('scripts')

      </body>
      </html>
      
## Updating & removing from the cart
Now to remove or update an item in the cart we will create two actions in the **ProductsController** as follows:

          public function update(Request $request)
            {
                if($request->id and $request->quantity)
                {
                    $cart = session()->get('cart');

                    $cart[$request->id]["quantity"] = $request->quantity;

                    session()->put('cart', $cart);

                    session()->flash('success', 'Cart updated successfully');
                }
            }

            public function remove(Request $request)
            {
                if($request->id) {

                    $cart = session()->get('cart');

                    if(isset($cart[$request->id])) {

                        unset($cart[$request->id]);

                        session()->put('cart', $cart);
                    }

                    session()->flash('success', 'Product removed successfully');
                }
            }
            
Let’s create two other routes            

      Route::patch('update-cart', 'ProductsController@update');

      Route::delete('remove-from-cart', 'ProductsController@remove');

Now go to resources>views>cart.blade.php and add this code at the bottom

      @section('scripts')


          <script type="text/javascript">

              $(".update-cart").click(function (e) {
                 e.preventDefault();

                 var ele = $(this);

                  $.ajax({
                     url: '{{ url('update-cart') }}',
                     method: "patch",
                     data: {_token: '{{ csrf_token() }}', id: ele.attr("data-id"), quantity: ele.parents("tr").find(".quantity").val()},
                     success: function (response) {
                         window.location.reload();
                     }
                  });
              });

              $(".remove-from-cart").click(function (e) {
                  e.preventDefault();

                  var ele = $(this);

                  if(confirm("Are you sure")) {
                      $.ajax({
                          url: '{{ url('remove-from-cart') }}',
                          method: "DELETE",
                          data: {_token: '{{ csrf_token() }}', id: ele.attr("data-id")},
                          success: function (response) {
                              window.location.reload();
                          }
                      });
                  }
              });

          </script>

      @endsection
      
As shown above we added two listeners for buttons update and remove and for each button just send ajax request to update and remove products.      
