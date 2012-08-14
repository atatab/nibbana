fs = require 'fs'
path = require 'path'
cs = require 'coffee-script'
rimraf = require 'rimraf'
browserify = require 'browserify'
mocha = require 'mocha'

spawn = require( "child_process" ).spawn

files = [ \
'storage/local.coffee',
'storage/object.coffee',
'tag/abstract.coffee',
'tag/area.coffee',
'tag/contact.coffee',
'tag/context.coffee',
'tag/tag.coffee',
'task/abstract.coffee',
'task/project.coffee',
'task/task.coffee',
'view/factory.coffee',
'view/filters.coffee',
'view/groupers.coffee',
'view/sorters.coffee',
'view/view.coffee',
'constants.coffee',
'cors_hack.coffee',
'entity_map.coffee',
'nibbana.coffee',
'session.coffee',
'util.coffee'
]

task 'build', 'Build nibbana for NPM', ( options ) ->
  if fs.existsSync( 'lib/' )
    rimraf.sync( 'lib' )
    fs.mkdirSync( 'lib' )
  for f in files
    in_file = "src/#{f}"
    out_file = "lib/#{f.replace( /coffee$/, 'js' )}"
    opts =
      filename: in_file
      header: true
    console.log( "Compiling #{in_file} to #{out_file}")
    dir = path.dirname( out_file )
    fs.mkdirSync( dir ) if ! fs.existsSync( dir )
    fs.writeFileSync( out_file, cs.compile( fs.readFileSync( in_file ).toString(), opts ) )

task 'browserify', 'Build nibbana for the browser', ( options ) ->
  b = browserify()
  b.require( 'jquery-browserify', target: 'jquery' )
  b.require( 'nibbana' )

task 'test', 'Run tests', ( options ) ->
  cmd = "node"
  options = [ "node_modules\\mocha\\bin\\mocha", "--compilers", "coffee:coffee-script", "--reporter", "spec", "--require", "coffee-script" ]
  spawn cmd, options, { customFds: [0,1,2] }