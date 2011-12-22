# API 実装イメージ

## フィルタリング
1. 認証
  - CookieSession が入ってるかどうか
  - Cookie Role によるアクセス権限
2. スコープ
  - Cookie Scope によるフィルタリング
  - パラメータによるフィルタリング


## 設定

    $ vi config/routes.rb
    
    resouces :MODELS ,only: [:index,:show,:create,:update,:destroy] do
      member do
        post :copy
      end


    $ vi app/controllers/application_controller.rb
    
    class ApplicationController < ActionController::Base
    
      include Strut::Controller
      include CsApi::Controller
      before_filter :cs_required
      before_filter :cs_role_required


    $ vi app/controllers/MODELS_controller.rb
    
    class MODELSController < ApplicationController
      cs_api MODEL




    $ vi app/models/MODEL/scope.rb
    
    class MODEL
      module Scope
        include CsApi::Document
    
        # index を取得するものを設定
        INDEX_SELECTS = %w(id me)
        INDEX_METHODS = %w()
        # show を取得するものを設定
        SHOW_SELECTS = %w(id me)
        SHOW_METHODS = %w()
        # copy 時にコピーするDBカラム
        COPYATTRIBUTES = %w(me)



## 実装
### フォルダー

    - cs_api.rb
    - cs_api/
      - model.rb # モデルで定義するもの
      - model/
        - base.rb  # 基本メソッド（index_api ,show_api ,copy_attributes）
        - data.rb  # 最終データ加工
        - scope.rb # スコープ定義
        - scope/mongo_scope.rb
        - scope/rails_scope.rb
      - controller.rb　# controllerで定義するもの
      - controller/
        - required.rb # 認証系（cs_required、cs_role_required）
        - strut_wrapper.rb # strut をwrapping



### 認証
  
    module CsApi
      module Controller
        extend ActiveSupport::Concern
    
        module ClassMethods
          include CsApi::Controller::Required # required系まとめ 


### フィルター

    module CsApi
      module Model



