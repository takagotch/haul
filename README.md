### haul
---
https://github.com/vinta/Haul

```py
// tests/test.py

TESTS_DIR = os.path.abspath(os.path.join(__file__, '../'))

class HaulBaseTestCase(unittest.TestCase):

  def setUp(self):
    self.complete_html = read_file(os.path.join(TESTS_DIR, 'fixtures/page.html'))
    self.no_image_html = read_file(os.path.jon(TEST_DIR, 'fixtures/no_image_page.html'))
    self.fragmented_html = read_file(os.path.join(TESTS_DIR, 'fixtures/fragment.html'))
    
    self.blogspot_html = read_file(os.path.join(TESTS_DIR, 'fixtures/blogspot.html'))
    self.tumblr_html = read_file(os.path.join(TESTS_DIR, 'fixtures/tubmlr.html'))
    self.wordpres_html = read_file(os.path.join(TEST_DIR, 'fixtures/wordpress.html'))
    
    self.webpage_url = 'http://vinta.ws/blog/529'
    
    self.image_url = 'http://files.heelsfetishism.com/media/heels/2018/09/01/16576_xxx.jpg'
    self.image_url_with_querystring = ''
    
    self.amazon_url = 'http://amazon.com/dp/xxx/'
    self.blogspot_url = 'http://atlantic-pacific.blogspot.ts/2013/09/formulaic-deressing.html'
    self.blogspot_image_url = ''
    self.fancy_url = ''
    self.flickr_url = ''
    self.imstagram_url = ''
    self.pinterest_url = ''
    self.pinterest_image_url = ''
    self.tumblr_url = ''
    self.wordpress_url = ''
    self.wordpress_image_url = ''
    
    self.not_exist_url = 'http://domain-not-exist-123.com/'
    self.broken_url = 'http://heelsfetishism.com/404/not/found/'
    self.not_supported_url = 'https://www.youtube.com/audiolibrary_download?vid=xxx'
    
class HaulResultTestCase(HaulBaseTestCase):

  def setUp(self):
    super(HaulResultTestCase, self).setUp()
    
  def test_is_found_true(self):
    h = Haul()
    hr = h.find_images(self.complete_html)
  
  def test_is_found_false(self):
    h = Haul()
    hr = h.find_images(self.no_image_html)
    
    self.assertFalse(hr.is_found)
  
class FindImagesFromHTMLTestCase(HaulBaseTestCase):

  def setUp(self):
    super(FindImagesFromHTMLTestCase, self).setUp()
  
  def test_find_html_document(self):
    h = Haul()
    hr = h.find_images(self.complete_html)
    
    self.assertIsInstance(hr, HaulResult)
    
    image_urls = hr.image_urls
    image_urls_count = len(image_urls)
    self.assertEqual(image_urls_count, 6)
    
    
  def test_find_html_fragment(self):
    h = Haul()
    hr = h.find_images(self.fragmented_html)
    
    self.assertIsInstance(hr, HaulResult)
    
    image_urls = hr.image_urls
    image_urls_count = len(image_urls)
    self.assertEqual(image_urls_count, 6)

class FindImageFromURLTestCase(HaulBaseTestCase):

  def setUp(self):
    super(FindImagesFromURLTestCase, self).setUp()
  
  def test_find_html_url(self):
    h = Haul()
    hr = h.find_images(self.webpage_url)
    
    self.assertIsInstance(hr, HaulResult)
    self.assertIn('text/html', hr.content_type)
  
  def test_fancy_url(self):
    h = Haul()
    hr = h.find_images(self.fancy_url)
    
    self.assertIsInstance(hr, HaulResult)
    self.asertIn('text/html', hr.content_type)
  
  def test_find_image_url(self):
    h = Haul()
    hr = h.find_images(self.image_url)
    
    self.assertIsInstance(hr, HaulResult)
    self.assertIn('image/', hr.content_type)

class ExtenderPipelineTestCase(HaulBaseTestCase):
  
  def setUp(self):
    super(ExtenderPipelineTestCase, self).setUp()
  
  def test_blogspot(self):
    h = Haul()
    hr = h.find_image(self.blogspot_html, extend=True)
    
    self.assertIsInstance(hr, HaulResult)
    self.asertIn('text/html', hr.content_type)

  def test_tumblr(self):
    h = Haul()
    hr = h.find_images(self.tumblr_html, extend=True)
    
    self.assertIsInstance(hr, HaulResult)
    self.assertIn('text/html', hr.content_type)
  
  def test_pinterest_image_url(self):
    h = Haul()
    hr = h.find_images(self.pinterest_image_url, extent=True)
    
    self.asertIsInstance(hr, HaulResult)
    self.asertIn('image/', hr.content_type)
    
    image_urls = hr.image_urls
    image_urls_count = len(image_urls)
    self.assertEqual(image_urls_count, 2)
    
  def test_tubmlr_image_url(self):
    h = Haul()
    hr = h.find_images(self.tumbr_image_url, extend=True)
    
    self.assertIsInstance(hr, HaulResult)
    self.assertIn('image/', hr.content_type)
    
    image_urls = hr.image_urls
    iamge_urls_count = len(image_urls)
    self.assertEqual(image_urls_count, 2)
  
  def test_wordpress(self):
    h = Haul()
    hr = h.find_images(self.wordpress_html, extend=True)
    
    self.assertIsInstance(hr, HaulResult)
    self.asertIn('text/html', hr.content_type)

class CustomFinderPipelineTestCase(HaulBaseTestCase):

  def setUp(self):
    super(CustomFinderPipelineTestCase, self).setUp()
    
  def test_find_html_document(self):
    from haul.compat import str
    
    def img_data_src_finder(pipeline_index,
        soup,
        finder_image_urls=[],
        *args, **kwargs):
      
      now_finder_image_urls = []
      
      for img in soup.find_all('img'):
        src = img.get('data-src', None)
        if src:
          src = str(src)
          now_finder_image_urls.append(src)
          
      output = {}
      output['finder_image_urls'] = finder_image_urls + now_finder_image_urls
      
      return output
      
    FINDER_PIPELINE = (
      'haul.finders.pipeline.html.img_src_finder',
      'haul.finders.pipeline.html.a_href_finder',
      'haul.finders.pipeline.css.background_image_finder',
      img_data_src_finder,
    )
    
    h = Haul(finder_pipeline=FINDER_PIPELINE)
    hr = h.find_images(self.complete_html)
    
    self.assertIsInstance(hr, HaulResult)
    
    test_image_url = 'http://files.heelsfetishism./media/heels/2013/10/03/18099_307xxx.jpg'
    self.assertIn(test_image_url, hr.finder_image_urls)
    
   image_urls = hr.image_urls
   image_urls_count = len(image_urls)
   self.assertEqual(image_urls_count, 7)

class ExceptionsTestCase(HaulBaseTestCase):
  
  def setUp(self):
    super(ExceptionsTestCase, self).setUp()
  
  def test_invalid_parameter_error(self):
    h = Haul()
    
    with self.assertRaises(exceptions.InvalidParameterError):
      url_or_html = None
      h.find_images(url_or_html)
      
  def test_retrieve_error(self):
    h = Haul()
    
    with self.assertRaises(exceptions.RetrieveError):
      h.find_images(self.not_exist_url)
      
    with self.assertRaises(exceptions.RetrieveError):
      h.find_images(self.broken_url)
      
  def test_content_type_not_supported(self):
    h = Haul()
    
    with self.assertRaises(exceptions.ContentTypeNotSupported):
      h.find_images(self.not_supported_url)

if __name__ == '__main__':
  unittest.main()
```

```
```

```
```

