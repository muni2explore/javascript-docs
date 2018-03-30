# javascript-docs

# bind example

<pre>
var makeReuest = function(url,cb){
  var data = 30;
  
  cb(data);
}

var obj = {
  amt: 50,
  loadData: function(data){
    var sum = this.amt + data;
    console.log(sum);
  },
  prepareRequest: function(){
    var url = "http://example.com";
    
    makeReuest(url, this.loadData.bind(this));
  }
};

obj.prepareRequest();
</pre>
