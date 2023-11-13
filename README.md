s shipping address

Product has many orders

Address belongs to a customer

Migration

Create customers table

create_table :customers do |t| t.string :name t.string :phone t.string :email t.string :address_line_1 t.string :address_line_2 t.string :city t.string :state t.string :country t.string :pin end

Create orders table

create_table :orders do |t| t.integer :customer_id t.string :order_number t.string :sku t.date :date_of_order t.string :order_status t.integer :billing_address_id t.integer :shipping_address_id end

Create products table

create_table :products do |t| t.integer :order_id t.string :name t.integer :mrp t.integer :quantity t.integer :discount t.integer :net_price end

Create addresses table

create_table :addresses do |t| t.integer :customer_id t.string :address_line_1 t.string :address_line_2 t.string :city t.string :state t.string :country t.string :pin end

Model

class Customer < ApplicationRecord has_many :orders has_one :address end

class Order < ApplicationRecord belongs_to :customer has_many :products has_one :billing_address has_one :shipping_address end

class Product < ApplicationRecord belongs_to :order end

class Address < ApplicationRecord belongs_to :customer end

Controller

class CustomersController < ApplicationController def index @customers = Customer.all end def show @customer = Customer.find(params[:id]) end def new @customer = Customer.new end def create @customer = Customer.new(customer_params) if @customer.save redirect_to @customer else render 'new' end end def edit @customer = Customer.find(params[:id]) end def update @customer = Customer.find(params[:id]) if @customer.update(customer_params) redirect_to @customer else render 'edit' end end def destroy @customer = Customer.find(params[:id]) @customer.destroy redirect_to customers_path end private def customer_params params.require(:customer).permit(:name, :phone, :email, :address_line_1, :address_line_2, :city, :state, :country, :pin) end end

class OrdersController < ApplicationController def index @orders = Order.all end def show @order = Order.find(params[:id]) end def new @order = Order.new end def create @order = Order.new(order_params) if @order.save redirect_to @order else render 'new' end end def edit @order = Order.find(params[:id]) end def update @order = Order.find(params[:id]) if @order.update(order_params) redirect_to @order else render 'edit' end end def destroy @order = Order.find(params[:id]) @order.destroy redirect_to orders_path end private def order_params params.require(:order).permit(:customer_id, :order_number, :sku, :date_of_order, :order_status, :billing_address_id, :shipping_address_id) end end

class ProductsController < ApplicationController def index @products = Product.all end def show @product = Product.find(params[:id]) end def new @product = Product.new end def create @product = Product.new(product_params) if @product.save redirect_to @product else render 'new' end end def edit @product = Product.find(params[:id]) end def update @product = Product.find(params[:id]) if @product.update(product_params) redirect_to @product else render 'edit' end end def destroy @product = Product.find(params[:id]) @product.destroy redirect_to products_path end private def product_params params.require(:product).permit(:order_id, :name, :mrp, :quantity, :discount, :net_price) end end

class AddressesController < ApplicationController def index @addresses = Address.all end def show
