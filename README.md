### haul
---
https://github.com/vinta/Haul

```py
// tests/test.py

TESTS_DIR = os.path.abspath(os.path.join(__file__, '../'))

class HaulBaseTestCase(unittest.TestCase):

  def setUp(self):
    self.complete_html = read_file(os.path.join(TESTS_DIR, 'fixtures/page.html'))
    
    
class HaulResultTestCase(HaulBaseTestCase):

  def setUp(self):
  
  def test_is_found_true():
  
  def test_is_found_false():
  
class FindImagesFromHTMLTestCase(HaulBaseTestCase):

  def setUp(self):
  
  def test_find_html_document(self):
  
  def test_find_html_fragment(self):

class FindImageFromURLTestCase(HaulBaseTestCase):











```

```
```

```
```

