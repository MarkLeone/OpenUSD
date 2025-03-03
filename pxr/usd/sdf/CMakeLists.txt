set(PXR_PREFIX pxr/usd)
set(PXR_PACKAGE sdf)

pxr_library(sdf
    LIBRARIES
        arch
        tf
        gf
        trace
        vt
        work
        ar
        ${Boost_PYTHON_LIBRARY}

    INCLUDE_DIRS
        ${Boost_INCLUDE_DIRS}

    PUBLIC_CLASSES
        abstractData
        allowed
        assetPath
        attributeSpec
        changeBlock
        changeList
        children
        childrenPolicies
        childrenProxy
        childrenUtils
        childrenView
        cleanupEnabler
        copyUtils
        data
        declareHandles
        fileFormat
        identity
        layer
        layerOffset
        layerStateDelegate
        layerTree
        layerUtils
        listProxy
        listEditor
        listEditorProxy
        listOp
        mapEditProxy
        mapEditor
        namespaceEdit
        notice
        opaqueValue
        path
        pathExpression
        pathExpressionEval
        pathNode
        pathTable
        payload
        pool
        predicateExpression
        predicateLibrary
        primSpec
        propertySpec
        proxyPolicies
        proxyTypes
        pseudoRootSpec
        reference
        relationshipSpec
        schema
        site
        siteUtils
        spec
        specType
        textFileFormat
        timeCode
        tokens
        types
        valueTypeName
        variableExpression
        variantSetSpec
        variantSpec

        # Flex/Bison generated code
        textFileFormat.tab
   
    PYTHON_PUBLIC_CLASSES
        pyChildrenProxy
        pyChildrenView
        pyListEditorProxy
        pyListOp
        pyListProxy
        pyMapEditProxy
        pySpec
        pyUtils

    PUBLIC_HEADERS
        api.h
        accessorHelpers.h
        declareSpec.h
        invoke.hpp
        layerHints.h
        predicateExpressionParser.h
        predicateProgram.h
        schemaTypeRegistration.h

    PRIVATE_CLASSES
        assetPathResolver
        changeManager
        cleanupTracker
        connectionListEditor
        debugCodes
        fileFormatRegistry
        fileIO
        fileIO_Common
        layerRegistry
        listOpListEditor
        parserHelpers
        parserValueContext
        subLayerListEditor
        textParserContext
        valueTypeRegistry
        variableExpressionImpl
        variableExpressionParser
        vectorListEditor

    PRIVATE_HEADERS
        instantiatePool.h
        valueTypePrivate.h

    CPPFILES
        # Flex/Bison generated code
        textFileFormat.lex.cpp

    PYTHON_CPPFILES
        moduleDeps.cpp

    PYMODULE_CPPFILES
        module.cpp
        wrapArrayAssetPath.cpp
        wrapArrayPath.cpp
        wrapArrayTimeCode.cpp
        wrapAssetPath.cpp
        wrapAttributeSpec.cpp
        wrapChangeBlock.cpp
        wrapCleanupEnabler.cpp
        wrapCopyUtils.cpp
        wrapFileFormat.cpp
        wrapLayer.cpp
        wrapLayerOffset.cpp
        wrapLayerTree.cpp
        wrapNamespaceEdit.cpp
        wrapNotice.cpp
        wrapOpaqueValue.cpp
        wrapPath.cpp
        wrapPathExpression.cpp
        wrapPayload.cpp
        wrapPredicateExpression.cpp
        wrapPrimSpec.cpp
        wrapPropertySpec.cpp
        wrapPseudoRootSpec.cpp
        wrapRelationshipSpec.cpp
        wrapReference.cpp
        wrapSpec.cpp
        wrapVariableExpression.cpp
        wrapTimeCode.cpp
        wrapTypes.cpp
        wrapValueTypeName.cpp
        wrapVariantSpec.cpp
        wrapVariantSetSpec.cpp

    PYMODULE_FILES 
        __init__.py

    RESOURCE_FILES
        plugInfo.json

    DOXYGEN_FILES
        overview.dox
        doxygen/fileFormatPlugin.dox
        doxygen/variableExpressions.dox
)

# Validate that the checked in generated code matches the sources. This requires
# a specific version of bison/flex to get the same results
if (NOT ${PXR_VALIDATE_GENERATED_CODE})
    message(STATUS "Skipping validation of sdf generated code because PXR_VALIDATE_GENERATED_CODE=OFF")
elseif(NOT BISON_FOUND)
    message(STATUS "Skipping validation of sdf generated code because of missing dependency: bison")
elseif(NOT FLEX_FOUND)
    message(STATUS "Skipping validation of sdf generated code because of missing dependency: flex")
else()
    add_custom_command(
        TARGET sdf
        PRE_BUILD 
        COMMENT "Validating sdf generated code..."
        COMMAND
            ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/sdfGenAsciiParsers.py 
            --srcDir ${CMAKE_CURRENT_SOURCE_DIR}
            --bison ${BISON_EXECUTABLE}
            --flex ${FLEX_EXECUTABLE}
            --bases textFileFormat
            --validate
    )
endif()

pxr_test_scripts(
    testenv/testSdfAssetPath.py
    testenv/testSdfAttribute.py
    testenv/testSdfAttributeBlocking.py
    testenv/testSdfBatchNamespaceEdit.py
    testenv/testSdfFileFormatCapabilities.py
    testenv/testSdfCopyUtils.py
    testenv/testSdfCustomLayerData.py
    testenv/testSdfDetachedLayer.py
    testenv/testSdfFileFormat.py
    testenv/testSdfLayer.py
    testenv/testSdfLayerMuting.py
    testenv/testSdfListOp.py
    testenv/testSdfParsing.py
    testenv/testSdfPath.py
    testenv/testSdfPath2.py
    testenv/testSdfPath2Construct.py
    testenv/testSdfPayload.py
    testenv/testSdfPrim.py
    testenv/testSdfReference.py
    testenv/testSdfSpecHash.py
    testenv/testSdfTimeCode.py
    testenv/testSdfTypes.py
    testenv/testSdfVariableExpression.py
    testenv/testSdfVariants.py
)

pxr_build_test_shared_lib(TestSdfFileFormatCapabilities
    CREATE_FRAMEWORK
    INSTALL_PREFIX SdfPlugins
    LIBRARIES
        sdf
        tf

    CPPFILES
        testenv/TestSdfFileFormatCapabilities.cpp
)

pxr_build_test_shared_lib(TestSdfNoAssetFileFormat
    CREATE_FRAMEWORK
    INSTALL_PREFIX SdfPlugins
    LIBRARIES
        sdf
        tf

    CPPFILES
        testenv/TestSdfNoAssetFileFormat.cpp
)

pxr_build_test_shared_lib(TestSdfStreamingFileFormat
    CREATE_FRAMEWORK
    INSTALL_PREFIX SdfPlugins
    LIBRARIES
        sdf
        tf

    CPPFILES
        testenv/TestSdfStreamingFileFormat.cpp
)

pxr_build_test_shared_lib(TestSdfTargetFileFormat
    CREATE_FRAMEWORK
    INSTALL_PREFIX SdfPlugins
    LIBRARIES
        sdf
        tf

    CPPFILES
        testenv/TestSdfTargetFileFormat.cpp
)

pxr_build_test_shared_lib(TestSdfResolver
    CREATE_FRAMEWORK
    INSTALL_PREFIX SdfPlugins
    LIBRARIES
        ar
        sdf
        tf

    CPPFILES
        testenv/TestSdfResolver.cpp
)

pxr_build_test(testSdfAttributeBlocking_Cpp
    LIBRARIES
        sdf
    CPPFILES
        testenv/testSdfAttributeBlocking.cpp
) 

pxr_build_test(testSdfHardToReach
    LIBRARIES
        sdf
    CPPFILES
        testenv/testSdfHardToReach.cpp
)

pxr_build_test(testSdfLayerHints
    LIBRARIES
        sdf
    CPPFILES
        testenv/testSdfLayerHints.cpp
)

pxr_build_test(testSdfMetaDataPlugInfo
    LIBRARIES
        sdf
    CPPFILES
        testenv/testSdfMetaDataPlugInfo.cpp
)

pxr_build_test(testSdfPathExpression_Cpp
    LIBRARIES
        sdf
    CPPFILES
        testenv/testSdfPathExpression.cpp
)

pxr_build_test(testSdfPathParser
    LIBRARIES
        sdf
    CPPFILES
        testenv/testSdfPathParser.cpp
)

pxr_build_test(testSdfPathTable
    LIBRARIES
        sdf
    CPPFILES
        testenv/testSdfPathTable.cpp
)

pxr_build_test(testSdfPathThreading
    LIBRARIES
        sdf
        tf
    CPPFILES
        testenv/testSdfPathThreading.cpp
)

pxr_build_test(testSdfPredicateExpression_Cpp
    LIBRARIES
        sdf
        tf
    CPPFILES
        testenv/testSdfPredicateExpression.cpp
)

pxr_install_test_dir(
    SRC testenv/testSdfBatchNamespaceEdit.testenv
    DEST testSdfBatchNamespaceEdit/testSdfBatchNamespaceEdit.testenv
)

pxr_install_test_dir(
    SRC testenv/testSdfCustomLayerData.testenv
    DEST testSdfCustomLayerData/testSdfCustomLayerData.testenv
)

pxr_install_test_dir(
    SRC testenv/testSdfLayerHints.testenv
    DEST testSdfLayerHints/testSdfLayerHints.testenv
)

pxr_install_test_dir(
    SRC testenv/testSdfLayerMuting.testenv
    DEST testSdfLayerMuting/testSdfLayerMuting.testenv
)

pxr_install_test_dir(
    SRC testenv/testSdfMetaDataPlugInfo.testenv
    DEST testSdfMetaDataPlugInfo/testSdfMetaDataPlugInfo.testenv
)

pxr_install_test_dir(
    SRC testenv/testSdfParsingOld.testenv
    DEST testSdfParsingOld/testSdfParsing.testenv
)

pxr_install_test_dir(
    SRC testenv/testSdfParsing.testenv
    DEST testSdfParsing/testSdfParsing.testenv
)

pxr_register_test(testSdfAssetPath
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfAssetPath"
)

pxr_register_test(testSdfAttribute
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfAttribute"
)

pxr_register_test(testSdfAttributeBlocking
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfAttributeBlocking"
)

pxr_register_test(testSdfAttributeBlocking_Cpp
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfAttributeBlocking_Cpp"
)

pxr_register_test(testSdfBatchNamespaceEdit
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfBatchNamespaceEdit"
)

pxr_register_test(testSdfCopyUtils
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfCopyUtils"
)

pxr_register_test(testSdfCustomLayerData
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfCustomLayerData"
)

pxr_register_test(testSdfDetachedLayer
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfDetachedLayer"
)

pxr_register_test(testSdfDetachedLayerEnvVar1
    PYTHON
    ENV
        SDF_LAYER_INCLUDE_IN_MEMORY=*
        SDF_LAYER_EXCLUDE_IN_MEMORY=foo,bar
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfDetachedLayer"
)

pxr_register_test(testSdfDetachedLayerEnvVar2
    PYTHON
    ENV
        SDF_LAYER_INCLUDE_IN_MEMORY=foo,bar
        SDF_LAYER_EXCLUDE_IN_MEMORY=baz,garply
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfDetachedLayer"
)

pxr_register_test(testSdfHardToReach
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfHardToReach"
)

pxr_register_test(testSdfFileFormat
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfFileFormat"
)

pxr_register_test(testSdfFileFormatCapabilities
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfFileFormatCapabilities"
)

pxr_register_test(testSdfLayer
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfLayer"
)

pxr_register_test(testSdfLayerHints
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfLayerHints"
)

pxr_register_test(testSdfLayerMuting
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfLayerMuting"
)

pxr_register_test(testSdfListOp
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfListOp"
)

pxr_register_test(testSdfMetaDataPlugInfo
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfMetaDataPlugInfo"
)

pxr_register_test(testSdfPathExpression_Cpp
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfPathExpression_Cpp"
)

pxr_register_test(testSdfParsing
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfParsing"
)

pxr_register_test(testSdfParsingOld
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfParsing"
)

pxr_register_test(testSdfPath
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfPath"
)

pxr_register_test(testSdfPath2
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfPath2"
)
pxr_register_test(testSdfPath2Construct
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfPath2Construct"
)

pxr_register_test(testSdfPathParser
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfPathParser"
)

pxr_register_test(testSdfPathTable
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfPathTable"
)

pxr_register_test(testSdfPathThreading
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfPathThreading"
)

pxr_register_test(testSdfPredicateExpression_Cpp
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfPredicateExpression_Cpp"
)

pxr_register_test(testSdfPayload
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfPayload"
)
pxr_register_test(testSdfPrim
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfPrim"
)
pxr_register_test(testSdfReference
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfReference"
)
pxr_register_test(testSdfSpecHash
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfSpecHash"
)
pxr_register_test(testSdfTimeCode
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfTimeCode"
)
pxr_register_test(testSdfTypes
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfTypes"
)
pxr_register_test(testSdfVariableExpression
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfVariableExpression"
)
pxr_register_test(testSdfVariants
    PYTHON
    COMMAND "${CMAKE_INSTALL_PREFIX}/tests/testSdfVariants"
)
