# webapp_server_file

#### Define Server Language
```
#!/usr/bin/env python
# coding: latin-1
```

#### Page Navigation Objects
```
main_nav_html = '''
  <a href="../../one"><li id="oneNav">One &nbsp; <span class="picto">&#10024;</span></li></a>
  <a href="../../two"><li id="twoNav">Two &nbsp; <span class="picto">&#10024;</span></li></a>
  <a href="../../three"><li id="threeNav">Three &nbsp; <span class="picto">&#10024;</span></li></a>
'''

user_nav_html = '''
  <a href="../../user/nu"><li id="user_nuNav">Nu &nbsp; <span class="picto">&#9862;</span></li></a>
  <a href="../../user/xi"><li id="user_xiNav">Xi &nbsp; <span class="picto">&#9862;</span></li></a>
  <a href="../../user/om"><li id="user_omNav">Om&nbsp; <span class="picto">&#9862;</span></li></a>
'''
```

#### Front Page HTML Code
```
front_page_html = '''<style>

</style>
<div class="page_html">
  <header class="hi">
    <span class="color_b"> &#9673; Hi &nbsp; <span class="picto"> &#9992; &#9732; &#10024; &#9642; &#8901;</span></span>
  </header>
 
</div><!-- - /page_html - -->'''
```

#### System
```
# - System
import os
import urllib
import wsgiref.handlers
import webapp2
import json
# - 
from google.appengine.api import users
from google.appengine.api import mail
# -
from google.appengine.ext import db
from google.appengine.ext.webapp import template
from google.appengine.ext.webapp.util import run_wsgi_app
# -
from urlparse import urlparse
# -
import time
import datetime
```

### Public Site HTML Page Production using Python & webapp2
```
class renderHTML(webapp2.RequestHandler):
    def get(self):
```

#### Check URL
```
        page_address = self.request.uri
        url_path = urlparse(page_address)[2]
```

#### Main vs Sub Pages 
```
        url_path_first = url_path.split('/')[1]
        url_path_last = base = os.path.basename(page_address)
        if url_path_first == url_path_last:
            page_class = 'main_page'
        else:
            page_class = 'sub_page'
```

#### Check User
```
        user = users.get_current_user()
```

#### if Valid User
```
        if users.get_current_user():
          login_key = users.create_logout_url(self.request.uri)
          gate = 'Sign out'
          user_name = user.nickname()
```

#### if No User
```
        else:
          login_key = users.create_login_url(self.request.uri)
          gate = 'Sign in'
          user_name = 'No User'
```

#### Check Admin
```
        admin = ''
        if users.is_current_user_admin():
            admin = 'true'
```

#### Set Navigation Type
```
        main_nav = main_nav_html
        if url_path_first == 'user':
          main_nav = user_nav_html
```

#### Page Data Settings
```
        app = app_name
        page_name = 'Front Page'
        page_id = 'front_page'
        page_html = front_page_html
```

#### Individual Page Settings
```
        if url_path == 'one':
            page_id = 'one'
            page_name = 'One'
            page_html = form_page_html
        #-----------------------------#
        if url_path == 'two':
            page_id = 'two'
            page_name = 'Two'
            page_html = form_page_html
        #-----------------------------#
        if url_path == 'three':
            page_id = 'three'
            page_name = 'Three'
            page_html = form_page_html
```

#### User Page Settings
```
        if url_path_first == 'user':
            if url_path_last == 'nu':
                page_id = 'user_nu'
                page_name = 'User Nu'
                page_html = form_page_html
        #-----------------------------#
            if url_path_last == 'xi':
                page_id = 'user_xi'
                page_name = 'User Xi'
                page_html = form_page_html
        #-----------------------------#
            if url_path_last == 'om':
                page_id = 'user_om'
                page_name = 'User Om'
                page_html = form_page_html
```

#### Define HTML {{ Objects }} for Template File
```
        objects = {
            'Site': app,
            'login_key': login_key,
            'gate': gate,
            'user_name': user_name,
            'admin': admin,
          # -
            'page_id': page_id,
            'page_name': page_name,
            'page_html': page_html,
            'main_nav': main_nav,
        }
```

#### Compile and Send HTML File to Browser 
```
        path = os.path.join(os.path.dirname(__file__), 'browserFile.html')
        self.response.out.write(template.render(path, objects))
```

#### Database One
```
class One_db(db.Model):
    addTime = db.DateTimeProperty(auto_now_add=True)
    data_id = db.StringProperty()
  #
    data_a = db.StringProperty()
    data_b = db.StringProperty()
    data_c = db.StringProperty()
```

#### Adding New Data From URL '/add_one_db/?'
```
class addOne_db(webapp2.RequestHandler):
    def post(self):
        date_time = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
        data_id = date_time
        item = One_db(key_name=data_id)
        item.data_id = data_id
      # - -
        item.data_a = self.request.get('data_a')
        item.data_b = self.request.get('data_b')
        item.data_c = self.request.get('data_c')
      #
        item.put()
        time.sleep(1)
        self.redirect('/one')
```

#### Database Two
```
class Two_db(db.Model):
    addTime = db.DateTimeProperty(auto_now_add=True)
    data_id = db.StringProperty()
  #
    data_a = db.StringProperty()
    data_b = db.StringProperty()
    data_c = db.StringProperty()
```

#### Adding New Data From URL '/add_two_db/?'
```
class addTwo_db(webapp2.RequestHandler):
    def post(self):
        date_time = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
        data_id = date_time
        item = Two_db(key_name=data_id)
        item.data_id = data_id
      # - -
        item.data_a = self.request.get('data_a')
        item.data_b = self.request.get('data_b')
        item.data_c = self.request.get('data_c')
      #
        item.put()
        time.sleep(1)
        self.redirect('/two')
```

#### Database Three
```
class Three_db(db.Model):
    addTime = db.DateTimeProperty(auto_now_add=True)
    data_id = db.StringProperty()
  #
    data_a = db.StringProperty()
    data_b = db.StringProperty()
    data_c = db.StringProperty()
```

#### Adding New Data From URL '/add_three_db/?'
```
class addThree_db(webapp2.RequestHandler):
    def post(self):
        date_time = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
        data_id = date_time
        item = Three_db(key_name=data_id)
        item.data_id = data_id
      # - -
        item.data_a = self.request.get('data_a')
        item.data_b = self.request.get('data_b')
        item.data_c = self.request.get('data_c')
      #
        item.put()
        time.sleep(1)
        self.redirect('/three')
```

#### Deleteing Data
```
class deleteData(webapp2.RequestHandler):
    def get(self):
        page_address = self.request.uri
        base = os.path.basename(page_address)
        data_set = base.split('?')[1]
        data_id = base.split('?')[2]
        if data_set == 'one_db':
            item = db.Query(One_db).filter('data_id =', data_id).get()
        if data_set == 'two_db':
            item = db.Query(Two_db).filter('data_id =', data_id).get()
        if data_set == 'three_db':
            item = db.Query(Three_db).filter('data_id =', data_id).get()
        item.delete()
        time.sleep(1)
        self.redirect('../../user')
```

#### Query and List data as JSON 
```
class listData(webapp2.RequestHandler):
    def get(self):
        page_address = self.request.uri
        base = os.path.basename(page_address)
        data_set = base.split('?')[1]
        if data_set == 'one_db':
            q = db.Query(One_db)
        if data_set == 'two_db':
            q = db.Query(Two_db)
        if data_set == 'three_db':
            q = db.Query(Three_db)
        db_data = q.order('-addTime').fetch(100)
        data = []
        for f in db_data:
            data.append(db.to_dict(f))
        self.response.headers['Content-Type'] = 'application/json'
        self.response.out.write(json.dumps(data))
```

#### URL Routes  ,  Server Class
```
app = webapp2.WSGIApplication([
#--------- Front Page ----------#
    ('/', renderHTML),

#--------- Main Pages ----------#
    ('/one/?', renderHTML),
    ('/two/?', renderHTML),
    ('/three/?', renderHTML),

#--------- Sub Pages -----------#
    ('/one/form/?', renderHTML),
    ('/one/data/?', renderHTML),
    ('/two/form/?', renderHTML),
    ('/two/data/?', renderHTML),
    ('/three/form/?', renderHTML),
    ('/three/data/?', renderHTML),

#--------- User Pages ----------#
    ('/user/?', renderHTML),
    ('/user/nu/?', renderHTML),
    ('/user/xi/?', renderHTML),
    ('/user/om/?', renderHTML),


#------- Adding New Data -------#
    ('/add_one_db/?', addOne_db),
    ('/add_two_db/?', addTwo_db),
    ('/add_three_db/?', addThree_db),

#----- List & Delete Data ------#
    ('/list_data/?', listData),
    ('/delete_data/?', deleteData),
    

], debug=True)
```
